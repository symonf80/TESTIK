# card_post.xml

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"

    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">
    <ImageView
        android:id="@+id/avatar"
        android:layout_width="48dp"
        android:layout_height="48dp"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:srcCompat="@drawable/posta_avatar"
        android:importantForAccessibility="no" />

    <TextView
        android:id="@+id/author"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginStart="16dp"
        android:ellipsize="end"
        android:maxLines="1"
        app:layout_constraintBottom_toTopOf="@+id/published"
        app:layout_constraintEnd_toStartOf="@+id/menu"
        app:layout_constraintStart_toEndOf="@+id/avatar"
        app:layout_constraintTop_toTopOf="@+id/avatar"
        tools:text="@sample/posts.json/data/author" />

    <TextView
        android:id="@+id/published"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="16dp"
        app:layout_constraintBottom_toBottomOf="@+id/avatar"
        app:layout_constraintStart_toEndOf="@+id/avatar"
        app:layout_constraintTop_toBottomOf="@+id/author"
        tools:text="@sample/posts.json/data/published" />

    <androidx.constraintlayout.widget.Barrier
        android:id="@+id/barrier"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:barrierDirection="bottom"
        app:barrierMargin="16dp"
        app:constraint_referenced_ids="avatar,published" />

    <TextView
        android:id="@+id/content"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="16dp"
        android:autoLink="web"
        android:lineSpacingExtra="8dp"
        app:layout_constraintTop_toBottomOf="@id/barrier"
        tools:text="@string/text" />

    <ImageButton
        android:id="@+id/menu"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:background="@android:color/transparent"
        android:importantForAccessibility="no"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="@+id/avatar"
        app:srcCompat="@drawable/menu" />

    <androidx.constraintlayout.widget.Barrier
        android:id="@+id/barrier2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:barrierDirection="bottom"
        app:barrierMargin="16dp"
        app:constraint_referenced_ids="content" />

    <ImageButton
        android:id="@+id/likes"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:layout_marginTop="16dp"
        android:background="@android:color/transparent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/barrier2"
        app:srcCompat="@drawable/baseline_favorite_border_24"
        android:importantForAccessibility="no" />

    <TextView
        android:id="@+id/tvLikes"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintBottom_toBottomOf="@+id/likes"
        app:layout_constraintStart_toEndOf="@+id/likes"
        app:layout_constraintTop_toTopOf="@+id/likes"
        tools:text="1" />

    <ImageButton
        android:id="@+id/repost"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="50dp"
        android:background="@android:color/transparent"
        app:layout_constraintBottom_toBottomOf="@+id/likes"
        app:layout_constraintStart_toEndOf="@+id/tvLikes"
        app:layout_constraintTop_toTopOf="@+id/likes"
        app:srcCompat="@drawable/baseline_forward_to_inbox_24" />

    <TextView
        android:id="@+id/tvRepost"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="8dp"
        app:layout_constraintBottom_toBottomOf="@+id/repost"
        app:layout_constraintStart_toEndOf="@+id/repost"
        app:layout_constraintTop_toTopOf="@+id/repost"
        tools:text="1" />

    <TextView
        android:id="@+id/tvViews"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        tools:text="0"
        app:layout_constraintBottom_toBottomOf="@+id/tvRepost"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="@+id/tvRepost" />
</androidx.constraintlayout.widget.ConstraintLayout>

# activity_main.xml

<?xml version="1.0" encoding="utf-8"?>
<androidx.recyclerview.widget.RecyclerView xmlns:android="http://schemas.android.com/apk/res/android"

    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:padding="16dp"
    tools:listitem="@layout/card_post"
    app:layoutManager="androidx.recyclerview.widget.LinearLayoutManager"
    tools:context=".view.MainActivity">
</androidx.recyclerview.widget.RecyclerView>

# MainActivity

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
            adapter.list = posts
        }
        binding.root.adapter = adapter
    }
}

# PostsAdapter (в пакет adapter)

class PostsAdapter(
    private val onLike: (Post) -> Unit,
    private val onShare: (Post) -> Unit
) :
    RecyclerView.Adapter<PostViewHolder>() {
    var list: List<Post> = emptyList()
        set(value) {
            field = value
            notifyDataSetChanged()
        }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): PostViewHolder {
        val view = CardPostBinding.inflate(LayoutInflater.from(parent.context))
        return PostViewHolder(view, onLike, onShare)
    }

    override fun onBindViewHolder(holder: PostViewHolder, position: Int) {
        holder.bind(list[position])
    }

    override fun getItemCount() = list.size

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

# PostViewModel

class PostViewModel : ViewModel() {

    private val repository: PostRepository = PostRepositoryInMemoryImpl()
    val data = repository.getAll()
    fun likeById(id: Long) = repository.likeById(id)
    fun repost(id: Long) = repository.repost(id)
}

# PostRepositoryMemoryImpl

class PostRepositoryInMemoryImpl : PostRepository {

    private var posts = listOf(
        Post(
            id = 1,
            author = "Нетология. Университет интернет-профессий",
            content = "Привет,это новая Нетология!Когда-то Нетология начиналась с интенсивов по онлайн-маркетингу.Затем появились курсы по дизайну,разработке,аналитике и управлению.Мы растём сами и помогаем расти студентам: от новичков до уверенных профессионалов. Но самое важное остаётся с нами: мы верим,что в каждом уже есть сила,которая заставляет хотеть больше,целиться выше,бежать быстрее.Наша миссия - помочь встать на путь роста и начать цепочку перемен - http//netolo.gy/fyb",
            published = "21 мая в 18:36",
            likes = 999,
            repost = 555,
            views = 2100,
            likedByMe = false
        ),
        Post(
            id = 2,
            author = "Нетология. Университет интернет-профессий",
            content = "Привет,это новая Нетология!Когда-то Нетология начиналась с интенсивов по онлайн-маркетингу.Затем появились курсы по дизайну,разработке,аналитике и управлению.Мы растём сами и помогаем расти студентам: от новичков до уверенных профессионалов. Но самое важное остаётся с нами: мы верим,что в каждом уже есть сила,которая заставляет хотеть больше,целиться выше,бежать быстрее.Наша миссия - помочь встать на путь роста и начать цепочку перемен - http//netolo.gy/fyb",
            published = "22 мая в 18:36",
            likes = 888,
            repost = 7000,
            views = 2100,
            likedByMe = false
        ),
        Post(
            id = 3,
            author = "Нетология. Университет интернет-профессий",
            content = "Привет,это новая Нетология!Когда-то Нетология начиналась с интенсивов по онлайн-маркетингу.Затем появились курсы по дизайну,разработке,аналитике и управлению.Мы растём сами и помогаем расти студентам: от новичков до уверенных профессионалов. Но самое важное остаётся с нами: мы верим,что в каждом уже есть сила,которая заставляет хотеть больше,целиться выше,бежать быстрее.Наша миссия - помочь встать на путь роста и начать цепочку перемен - http//netolo.gy/fyb",
            published = "23 мая в 18:36",
            likes = 777,
            repost = 15998,
            views = 2100,
            likedByMe = false
        ),
        Post(
            id = 4,
            author = "Нетология. Университет интернет-профессий",
            content = "Привет,это новая Нетология!Когда-то Нетология начиналась с интенсивов по онлайн-маркетингу.Затем появились курсы по дизайну,разработке,аналитике и управлению.Мы растём сами и помогаем расти студентам: от новичков до уверенных профессионалов. Но самое важное остаётся с нами: мы верим,что в каждом уже есть сила,которая заставляет хотеть больше,целиться выше,бежать быстрее.Наша миссия - помочь встать на путь роста и начать цепочку перемен - http//netolo.gy/fyb",
            published = "24 мая в 18:36",
            likes = 666,
            repost = 7000,
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
}


