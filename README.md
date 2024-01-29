# PostViewModel

    val empty = Post(
    id = 0,
    content = "",
    author = "",
    likedByMe = false,
    published = "",
    likes = 0,
    repost = 0,
    views = 0
    )

    class PostViewModel : ViewModel() {

    private val repository: PostRepository = PostRepositoryInMemoryImpl()
    val data = repository.getAll()
    val edited = MutableLiveData(empty)

    fun changeContentAndSave(content: String) {
        edited.value?.let {
            if (content != it.content) {
                repository.save(it.copy(content = content))
            }
            edited.value = empty
        }
    }

    fun likeById(id: Long) = repository.likeById(id)
    fun repost(id: Long) = repository.repost(id)
    fun removeById(id: Long) = repository.removeById(id)
    fun edit(post: Post) {
        edited.value = post
    }
    }

 #  interface PostRepository {
   
    fun getAll(): LiveData<List<Post>>
    fun likeById(id: Long)
    fun repost(id: Long)
    fun removeById(id: Long)
    fun save(post: Post)
    }

#   class PostRepositoryInMemoryImpl : PostRepository {
   
    private var nextId: Long = 0
    private var posts = listOf(
        Post(
            id = nextId++,
            author = "Нетология. Университет интернет-профессий",
            content = "Привет,это новая Нетология!Когда-то Нетология начиналась с интенсивов по онлайн-маркетингу.Затем появились курсы по дизайну,разработке,аналитике и управлению.Мы растём сами и помогаем расти студентам: от новичков до уверенных профессионалов. Но самое важное остаётся с нами: мы верим,что в каждом уже есть сила,которая заставляет хотеть больше,целиться выше,бежать быстрее.Наша миссия - помочь встать на путь роста и начать цепочку перемен - http//netolo.gy/fyb",
            published = "21 мая в 18:36",
            likes = 999,
            repost = 555,
            views = 2100,
            likedByMe = false
        ),
        Post(
            id = nextId++,
            author = "Нетология. Университет интернет-профессий",
            content = "Привет,это новая Нетология!Когда-то Нетология начиналась с интенсивов по онлайн-маркетингу.Затем появились курсы по дизайну,разработке,аналитике и управлению.Мы растём сами и помогаем расти студентам: от новичков до уверенных профессионалов. Но самое важное остаётся с нами: мы верим,что в каждом уже есть сила,которая заставляет хотеть больше,целиться выше,бежать быстрее.Наша миссия - помочь встать на путь роста и начать цепочку перемен - http//netolo.gy/fyb",
            published = "22 мая в 18:36",
            likes = 888,
            repost = 333,
            views = 2100,
            likedByMe = false
        ),
        Post(
            id = nextId++,
            author = "Нетология. Университет интернет-профессий",
            content = "Привет,это новая Нетология!Когда-то Нетология начиналась с интенсивов по онлайн-маркетингу.Затем появились курсы по дизайну,разработке,аналитике и управлению.Мы растём сами и помогаем расти студентам: от новичков до уверенных профессионалов. Но самое важное остаётся с нами: мы верим,что в каждом уже есть сила,которая заставляет хотеть больше,целиться выше,бежать быстрее.Наша миссия - помочь встать на путь роста и начать цепочку перемен - http//netolo.gy/fyb",
            published = "23 мая в 18:36",
            likes = 777,
            repost = 222,
            views = 2100,
            likedByMe = false
        ),
        Post(
            id = nextId++,
            author = "Нетология. Университет интернет-профессий",
            content = "Привет,это новая Нетология!Когда-то Нетология начиналась с интенсивов по онлайн-маркетингу.Затем появились курсы по дизайну,разработке,аналитике и управлению.Мы растём сами и помогаем расти студентам: от новичков до уверенных профессионалов. Но самое важное остаётся с нами: мы верим,что в каждом уже есть сила,которая заставляет хотеть больше,целиться выше,бежать быстрее.Наша миссия - помочь встать на путь роста и начать цепочку перемен - http//netolo.gy/fyb",
            published = "24 мая в 18:36",
            likes = 666,
            repost = 111,
            views = 2100,
            likedByMe = false
        ),
    )
    private val data = MutableLiveData(posts)

    override fun getAll(): LiveData<List<Post>> = data

    override fun likeById(id: Long) {
        posts = posts.map {
            if (it.id != id) it else it.copy(
                likedByMe = !it.likedByMe,
                likes = if (it.likedByMe) it.likes - 1 else it.likes + 1
            )
        }
        data.value = posts
    }

    override fun repost(id: Long) {
        posts = posts.map {
            if (it.id != id) it else
                it.copy(repost = it.repost + 1)
        }
        data.value = posts
    }

    override fun removeById(id: Long) {
        posts = posts.filter { it.id != id }
        data.value = posts
    }

    override fun save(post: Post) {
        posts = if (post.id == 0L) {
            listOf(
                post.copy(id = nextId++, published = "Now", author = "Netology")
            ) + posts

        } else {
            posts.map {
                if (it.id != post.id) it else it.copy(content = post.content)
            }
        }
        data.value = posts
    }
    }

#  interface OnInteractionListener {
    
    fun onLike(post: Post)
    fun onShare(post: Post)
    fun onRemove(post: Post)
    fun onEdit(post: Post)
    }

    class PostsAdapter(
    private val onInteractionListener: OnInteractionListener

    ) : ListAdapter<Post, PostViewHolder>(PostDiffCallback) {
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): PostViewHolder {
        val view = CardPostBinding.inflate(LayoutInflater.from(parent.context))
        return PostViewHolder(view, onInteractionListener)
    }

    override fun onBindViewHolder(holder: PostViewHolder, position: Int) {
        holder.bind(getItem(position))
    }
    }


    class PostViewHolder(
    private val binding: CardPostBinding,
    private val onInteractionListener: OnInteractionListener

    ) :
    RecyclerView.ViewHolder(binding.root) {
    private val service = Service()
    fun bind(post: Post) {
        with(binding) {
            author.text = post.author
            published.text = post.published
            content.text = post.content
            tvLikes.text = service.counter(post.likes)
            tvRepost.text = service.counter(post.repost)
            tvViews.text = service.counter(post.views)
            likes.setImageResource(
                if (post.likedByMe) R.drawable.baseline_favorite_24 else R.drawable.baseline_favorite_border_24
            )

            likes.setOnClickListener {
                onInteractionListener.onLike(post)
            }
            repost.setOnClickListener {
                onInteractionListener.onShare(post)
            }
            menu.setOnClickListener {
                PopupMenu(it.context, it).apply {
                    inflate(R.menu.options_post)
                    setOnMenuItemClickListener { item ->
                        when (item.itemId) {
                            R.id.remove -> {
                                onInteractionListener.onRemove(post)
                                true
                            }

                            R.id.edit -> {
                                onInteractionListener.onEdit(post)
                                true
                            }

                            else -> false
                        }

                    }
                }.show()
            }
        }
    }
    }

    object PostDiffCallback : DiffUtil.ItemCallback<Post>() {

    override fun areItemsTheSame(oldItem: Post, newItem: Post) = oldItem.id == newItem.id
    override fun areContentsTheSame(oldItem: Post, newItem: Post) = oldItem == newItem
    }

 #  class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
        val viewModel: PostViewModel by viewModels()
        val adapter = PostsAdapter(object : OnInteractionListener {
            override fun onLike(post: Post) {
                viewModel.likeById(post.id)
            }

            override fun onShare(post: Post) {
                viewModel.repost(post.id)
            }

            override fun onRemove(post: Post) {
                viewModel.removeById(post.id)
            }

            override fun onEdit(post: Post) {
                viewModel.edit(post)
            }

        })
        binding.list.adapter = adapter
        viewModel.data.observe(this) { posts ->
            val newPost = adapter.currentList.size < posts.size && adapter.currentList.size > 0
            adapter.submitList(posts) {
                if (newPost) {
                    binding.list.smoothScrollToPosition(0)
                }
            }
        }
        viewModel.edited.observe(this) { post ->
            if (post.id != 0L) {
                binding.edit.setText(post.content)
                binding.edit.focusAndShowKeyboard()
            }
        }
        binding.save.setOnClickListener {
            val text = binding.edit.text.toString().trim()
            if (text.isEmpty()) {
                Toast.makeText(this, "Пустое сообщение", Toast.LENGTH_SHORT).show()
                return@setOnClickListener
            }
            viewModel.changeContentAndSave(text)

            binding.edit.setText("")
            binding.edit.clearFocus()
            AndroidUtils.hideKeyboard(it)

        }
    }
    }

# object AndroidUtils {

    fun hideKeyboard(view: View) {
        val imm = view.context.getSystemService(Context.INPUT_METHOD_SERVICE) as InputMethodManager
        imm.hideSoftInputFromWindow(view.windowToken, 0)
    }

    fun View.focusAndShowKeyboard() {
        /**
         * This is to be called when the window already has focus.
         */
        fun View.showTheKeyboardNow() {
            if (isFocused) {
                post {
                    // We still post the call, just in case we are being notified of the windows focus
                    // but InputMethodManager didn't get properly setup yet.
                    val imm =
                        context.getSystemService(Context.INPUT_METHOD_SERVICE) as InputMethodManager
                    imm.showSoftInput(this, InputMethodManager.SHOW_IMPLICIT)
                }
            }
        }

        requestFocus()
        if (hasWindowFocus()) {
            // No need to wait for the window to get focus.
            showTheKeyboardNow()
        } else {
            // We need to wait until the window gets focus.
            viewTreeObserver.addOnWindowFocusChangeListener(
                object : ViewTreeObserver.OnWindowFocusChangeListener {
                    override fun onWindowFocusChanged(hasFocus: Boolean) {
                        // This notification will arrive just before the InputMethodManager gets set up.
                        if (hasFocus) {
                            this@focusAndShowKeyboard.showTheKeyboardNow()
                            // It’s very important to remove this listener once we are done.
                            viewTreeObserver.removeOnWindowFocusChangeListener(this)
                        }
                    }
                })
        }
    }
    }


# Group
    <androidx.constraintlayout.widget.Barrier
        android:id="@+id/barrierTop"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:barrierDirection="top"
        app:constraint_referenced_ids="close" />

    <androidx.constraintlayout.widget.Group
        android:id="@+id/group"
        android:layout_width="match_parent"
        android:layout_height="50dp"
        android:layout_marginBottom="24dp"
        android:background="@color/white"
        android:visibility="gone"
        app:constraint_referenced_ids="create,message,netol,close"
        app:layout_constraintBottom_toTopOf="@+id/edit" />

    <ImageView
        android:id="@+id/create"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="16dp"
        android:layout_marginBottom="16dp"
        android:background="@drawable/baseline_create_24"
        app:layout_constraintBottom_toBottomOf="@id/group"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@id/group" />

    <TextView
        android:id="@+id/message"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="16dp"
        android:text="Edit Message"
        android:textColor="#2B8AB5"
        app:layout_constraintBottom_toTopOf="@+id/netol"
        app:layout_constraintStart_toEndOf="@+id/create"
        app:layout_constraintTop_toTopOf="@+id/create" />

    <TextView
        android:id="@+id/netol"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="16dp"
        android:text="Нетология"

        app:layout_constraintBottom_toBottomOf="@+id/create"
        app:layout_constraintStart_toEndOf="@+id/create"
        app:layout_constraintTop_toBottomOf="@+id/message" />

    <ImageButton
        android:id="@+id/close"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:background="@drawable/baseline_close_24"
        app:layout_constraintBottom_toBottomOf="@id/group"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="@id/group" />
