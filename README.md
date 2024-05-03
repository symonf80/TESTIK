package ru.netology.nmedia.viewmodel

        import android.app.Application
        import android.widget.Toast
        import androidx.lifecycle.*
        import kotlinx.coroutines.launch
        import ru.netology.nmedia.R
        import ru.netology.nmedia.db.AppDb
        import ru.netology.nmedia.dto.Post
        import ru.netology.nmedia.model.FeedModel
        import ru.netology.nmedia.model.FeedModelState
        import ru.netology.nmedia.repository.PostRepository
        import ru.netology.nmedia.repository.PostRepositoryImpl
        import ru.netology.nmedia.util.SingleLiveEvent
        
        private val empty = Post(
            id = 0,
            content = "",
            author = "",
            authorAvatar = "",
            likedByMe = false,
            likes = 0,
            published = ""
        )

        class PostViewModel(application: Application) : AndroidViewModel(application) {
            // упрощённый вариант
            private val repository: PostRepository =
                PostRepositoryImpl(AppDb.getInstance(context = application).postDao())

    val data: LiveData<FeedModel> = repository.data.map(::FeedModel)
    private val _dataState = MutableLiveData<FeedModelState>()
    val dataState: LiveData<FeedModelState>
        get() = _dataState

    private val edited = MutableLiveData(empty)
    private val _postCreated = SingleLiveEvent<Unit>()
    val postCreated: LiveData<Unit>
        get() = _postCreated

    init {
        loadPosts()
    }

    fun loadPosts() = viewModelScope.launch {
        try {
            _dataState.value = FeedModelState(loading = true)
            repository.getAll()
            _dataState.value = FeedModelState()
        } catch (e: Exception) {
            _dataState.value = FeedModelState(error = true)
        }
    }

    fun refreshPosts() = viewModelScope.launch {
        try {
            _dataState.value = FeedModelState(refreshing = true)
            repository.getAll()
            _dataState.value = FeedModelState()
        } catch (e: Exception) {
            _dataState.value = FeedModelState(error = true)
        }
    }

    fun save() {
        edited.value?.let {
            _postCreated.value = Unit
            viewModelScope.launch {
                try {
                    repository.save(it)
                    _dataState.value = FeedModelState()
                } catch (e: Exception) {
                    _dataState.value = FeedModelState(error = true)
                }
            }
        }
        edited.value = empty
    }

    fun edit(post: Post) {
        edited.value = post
    }

    fun changeContent(content: String) {
        val text = content.trim()
        if (edited.value?.content == text) {
            return
        }
        edited.value = edited.value?.copy(content = text)
    }

    fun likeById(id: Long) {
        viewModelScope.launch {
            try {
                repository.likeById(id)
                _dataState.value = FeedModelState()
            } catch (e: Exception) {
                _dataState.value = FeedModelState(error = true)
            }
        }
    }

    fun removeById(id: Long) {
        viewModelScope.launch {
            try {
                repository.removeById(id)
                _dataState.value = FeedModelState()
            } catch (e: Exception) {
                _dataState.value = FeedModelState(error = true)
            }
        }
    }
        }

package ru.netology.nmedia.repository

        import androidx.lifecycle.*
        import okio.IOException
        import ru.netology.nmedia.api.*
        import ru.netology.nmedia.dao.PostDao
        import ru.netology.nmedia.dto.Post
        import ru.netology.nmedia.entity.PostEntity
        import ru.netology.nmedia.entity.toDto
        import ru.netology.nmedia.entity.toEntity
        import ru.netology.nmedia.error.ApiError
        import ru.netology.nmedia.error.NetworkError
        import ru.netology.nmedia.error.UnknownError
        
        class PostRepositoryImpl(private val dao: PostDao) : PostRepository {
            override val data = dao.getAll().map(List<PostEntity>::toDto)

    override suspend fun getAll() {
        try {
            val response = PostsApi.service.getAll()
            if (!response.isSuccessful) {
                throw ApiError(response.code(), response.message())
            }

            val body = response.body() ?: throw ApiError(response.code(), response.message())
            dao.insert(body.toEntity())
        } catch (e: IOException) {
            throw NetworkError
        } catch (e: Exception) {
            throw UnknownError
        }
    }

    override suspend fun save(post: Post) {
        try {
            val response = PostsApi.service.save(post)
            if (!response.isSuccessful) {
                throw ApiError(response.code(), response.message())
            }

            val body = response.body() ?: throw ApiError(response.code(), response.message())
            dao.insert(PostEntity.fromDto(body))
        } catch (e: IOException) {
            throw NetworkError
        } catch (e: Exception) {
            throw UnknownError
        }
    }

    override suspend fun removeById(id: Long) {
        try {
            dao.removeById(id)
            val response = PostsApi.service.removeById(id)
            if (!response.isSuccessful) {
                throw ApiError(response.code(), response.message())
            }
        } catch (e: IOException) {
            throw NetworkError
        } catch (e: Exception) {
            throw UnknownError
        }
    }

    override suspend fun likeById(id: Long) {
        try {
            data.value?.find { it.id == id }?.let {
                it.copy(
                    likedByMe = !it.likedByMe,
                    likes = it.likes + if (it.likedByMe) -1 else +1
                ).apply {
                    dao.insert(PostEntity.fromDto(this))
                    PostsApi.service.let { api ->
                        if (likedByMe) api.likeById(id) else api.dislikeById(id)
                    }.apply {
                        if (!isSuccessful) {
                            throw ApiError(code(), message())
                        }
                    }
                }
            } ?: throw UnknownError
        } catch (e: IOException) {
            throw NetworkError
        } catch (e: Exception) {
            throw UnknownError
        }
    }
        }

