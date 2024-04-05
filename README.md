postViewModel

     fun likeById(id: Long) {
        val currentPosts = _data.value?.posts.orEmpty()
        val currentPost = currentPosts.find { it.id == id }
        currentPost?.let { post ->
            val updatedPost = if (post.likedByMe) {
                post.copy(likes = post.likes - 1, likedByMe = false)
            } else {
                post.copy(likes = post.likes + 1, likedByMe = true)
            }
            val updatedPosts = currentPosts.map {
                if (it.id == id) updatedPost else it
            }
            _data.postValue(_data.value?.copy(posts = updatedPosts))

                if (post.likedByMe) {
                    repository.likeById(id, object : PostRepository.GetAllCallback<Post> {
                        override fun onError(e: Exception) {
                            _data.postValue(FeedModel(error = true))
                        }

                        override fun onSuccess(post: Post) {
                            _data.postValue(
                                FeedModel(posts = _data.value?.posts
                                    .orEmpty().map {
                                        if (it.id == post.id) post else it
                                    })
                            )
                        }
                    })
                } else {
                   // repository.likeById(id)
                }

        }
