data class Post(

    val id: Int,
    val author:String,
    val content:String,
    val published:String,
    val likes: Int,
    val repost:Int,
    val views:Int,
    val likedByMe: Boolean
)

interface PostRepository {

    fun get(): LiveData<Post>
    fun like()
    fun repost()
    fun views()
}

class PostRepositoryInMemoryImpl : PostRepository {

    private var post = Post(
        id = 1,
        author = "Нетология. Университет интернет-профессий",
        content = "Привет,это новая Нетология!Когда-то Нетология начиналась с интенсивов по онлайн-маркетингу.Затем появились курсы по дизайну,разработке,аналитике и управлению.Мы растём сами и помогаем расти студентам: от новичков до уверенных профессионалов. Но самое важное остаётся с нами: мы верим,что в каждом уже есть сила,которая заставляет хотеть больше,целиться выше,бежать быстрее.Наша миссия - помочь встать на путь роста и начать цепочку перемен - http//netolo.gy/fyb",
        published = "21 мая в 18:36",
        likes = 999,
        repost = 556,
        views = 12100,
        likedByMe = false
    )
    private val data = MutableLiveData(post)

    override fun get(): LiveData<Post> = data

    override fun like() {
        post = post.copy(
            likedByMe = !post.likedByMe,
            likes = if (post.likedByMe) post.likes - 1 else post.likes + 1
        )
        data.value = post
    }

    override fun repost() {
        post = post.copy(repost = post.repost + 1)
        data.value = post
    }

    override fun views() {
        post = post.copy(views = post.views)
        data.value = post
    }


}

class PostViewModel : ViewModel() {

    private val repository: PostRepository = PostRepositoryInMemoryImpl()
    val data = repository.get()
    fun like() = repository.like()
    fun repost() = repository.repost()
}

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
        val viewModel: PostViewModel by viewModels()

        val calc = Service()
       viewModel.data.observe(this) { post ->
            with(binding) {
                textView.text = post.author
                textView2.text = post.published
                text.text = post.content
                likes.setImageResource(
                    if (post.likedByMe) R.drawable.baseline_favorite_24 else R.drawable.baseline_favorite_border_24
                )
                tvLikes.text = calc.counter(post.likes)
                tvRepost.text = calc.counter(post.repost)
                tvViews.text = calc.counter(post.views)
                if (post.likedByMe) {
                    likes.setImageResource(R.drawable.baseline_favorite_24)
                }
              likes.setOnClickListener {
                    println("likes")
                    viewModel.like()

                }
             repost.setOnClickListener {
                    viewModel.repost()

                }
            }
        }

    }
}


dependencies {

    implementation("androidx.lifecycle:lifecycle-viewmodel-ktx:2.7.0")
    implementation("androidx.lifecycle:lifecycle-livedata-ktx:2.7.0")
    implementation("androidx.activity:activity-ktx:1.8.2")
    implementation("androidx.core:core-ktx:1.12.0")
    implementation("androidx.appcompat:appcompat:1.6.1")
    implementation("com.google.android.material:material:1.11.0")
    implementation("androidx.constraintlayout:constraintlayout:2.1.4")
    testImplementation("androidx.arch.core:core-testing:2.2.0")
    testImplementation("junit:junit:4.13.2")
    androidTestImplementation("androidx.test.ext:junit:1.1.5")
    androidTestImplementation("androidx.test.espresso:espresso-core:3.5.1")
}
