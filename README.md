class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
        val viewModel: PostViewModel by viewModels()
        val adapter = PostsAdapter ({
            viewModel.likeById(it.id)
        },{
            viewModel.repost(it.id)
        })

        viewModel.data.observe(this) { posts ->
            adapter.submitList(posts)
        }
        binding.root.adapter = adapter
    }
    }

class PostsAdapter(

    private val onLike: (Post) -> Unit,
    private val onShare: (Post) -> Unit
    ) : ListAdapter<Post, PostViewHolder>(PostDiffCallback) {
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): PostViewHolder {
        val view = CardPostBinding.inflate(LayoutInflater.from(parent.context))
        return PostViewHolder(view, onLike, onShare)
    }

    override fun onBindViewHolder(holder: PostViewHolder, position: Int) {
        holder.bind(getItem(position))
    }
    }


    class PostViewHolder(
    private val binding: CardPostBinding,
    private val onLike: (Post) -> Unit,
    private val onShare: (Post) -> Unit
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
                onLike(post)
            }
            repost.setOnClickListener {
                onShare(post)
            }
        }
    }
    }

    object PostDiffCallback : DiffUtil.ItemCallback<Post>() {

    override fun areItemsTheSame(oldItem: Post, newItem: Post) = oldItem.id == newItem.id
    override fun areContentsTheSame(oldItem: Post, newItem: Post) = oldItem == newItem
    }
