data class Post(
 
    val id: Long,
    val author:String,
    val content:String,
    val published:String,
    val likes: Int,
    val repost:Int,
    val views:Int,
    val likedByMe: Boolean,
    val video:String
    )


    ----------------------------------------
    val empty = Post(
     id = 0,
    content = "",
    author = "",
    likedByMe = false,
    published = "",
    likes = 0,
    repost = 0,
    views = 0,
    video = ""
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

    fun closeEdited(post: Post) {
        edited.value = empty
    }

    }

--------------------------------------------------------
    interface OnInteractionListener {
        fun onLike(post: Post)
    fun onShare(post: Post)
    fun onRemove(post: Post)
    fun onEdit(post: Post)
    fun onPlay(post: Post)

     }

     class PostsAdapter(
    private val onInteractionListener: OnInteractionListener

     ) : ListAdapter<Post, PostViewHolder>(PostDiffCallback) {
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): PostViewHolder {
        val view = CardPostBinding.inflate(LayoutInflater.from(parent.context), parent, false)
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
            likes.text = service.counter(post.likes)
            repost.text = service.counter(post.repost)
            tvViews.text = service.counter(post.views)
            likes.isChecked = post.likedByMe
            likes.setOnClickListener {
                onInteractionListener.onLike(post)
            }
            repost.setOnClickListener {
                onInteractionListener.onShare(post)
            }

            play.setOnClickListener {
                onInteractionListener.onPlay(post)
            }
            video.setOnClickListener {
                onInteractionListener.onPlay(post)
            }
            video.viewTreeObserver.apply {
                if (post.video.isNotEmpty() ) {
                    play.visibility = View.VISIBLE
                    video.visibility = View.VISIBLE
                }else{
                    play.visibility = View.GONE
                    video.visibility = View.GONE
                }
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

    ---------------------------------------------------------

    class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
        val viewModel: PostViewModel by viewModels()

        val newPostLauncher = registerForActivityResult(NewPostContract) { result ->
            result ?: return@registerForActivityResult
            viewModel.changeContentAndSave(result)


        }

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


            override fun onPlay(post: Post) {
                val intent = Intent(Intent.ACTION_VIEW, Uri.parse(post.video))
                val viewIntent = Intent.createChooser(intent, "My")
                startActivity(viewIntent)
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
                newPostLauncher.launch(post.content)

            }
        }
        binding.add.setOnClickListener {

            newPostLauncher.launch(null)
        }
    }
    }
    ----------------------------------------

     class NewPostActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val binding = ActivityNewPostBinding.inflate(layoutInflater)
        setContentView(binding.root)

        val editText = intent.getStringExtra(CONTENT_KEY)
        binding.edit.setText(editText)
        binding.edit.requestFocus()

        binding.ok.setOnClickListener {

            val text = binding.edit.text.toString()
            if (text.isNotBlank()) {
                setResult(RESULT_OK, Intent().apply { putExtra(NEW_POST_CONTENT_KEY, text) })
            } else {
                setResult(RESULT_CANCELED)
            }
            finish()
        }

        binding.bottomAppBar.setOnMenuItemClickListener { menuItem ->
            when (menuItem.itemId) {
                R.id.cancel -> {

                    finish()
                    true
                }

                else -> {

                    false
                }
            }
        }
    }

    companion object {
        const val CONTENT_KEY = "content"
        const val NEW_POST_CONTENT_KEY = "newPostContent"
        const val VIDEO_URL_KEY = "videoUrlContent"
    }

    }

    object NewPostContract : ActivityResultContract<String?, String?>() {
    override fun createIntent(context: Context, input: String?) =
        Intent(context, NewPostActivity::class.java).putExtra(CONTENT_KEY, input)


    override fun parseResult(resultCode: Int, intent: Intent?) =
        intent?.getStringExtra(NEW_POST_CONTENT_KEY)

    }
    -----------------------------------------------

    
