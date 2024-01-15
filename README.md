


class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
        val post = Post(
            id = 0,
            likes = 1899,
            repost = 1897,
            likedByMe = false
        )
        with(binding) {
            tvLikes.text = counter(post.likes)
            tvRepost.text=counter(post.repost)
            if (post.likedByMe) {
                likes.setImageResource(R.drawable.baseline_favorite_24)
            }

            likes.setOnClickListener {
                println("likes")
                if (post.likedByMe) post.likes-- else post.likes++
                post.likedByMe = !post.likedByMe
                tvLikes.text = counter(post.likes)
                likes.setImageResource(if (post.likedByMe) R.drawable.baseline_favorite_24 else R.drawable.baseline_favorite_border_24)

            }
            repost.setOnClickListener {
                post.repost++
                tvRepost.text = counter(post.repost)

            }
            root.setOnClickListener {
                println("root")
            }
            avatar.setOnClickListener {
                println("avatar")
            }
        }
    }

    private fun counter(item: Int): String {
        return when (item) {
            in 1000..1099 -> {
                val num = roundOffDecimal(item / 1000.0)
                (num + "K")
            }

            in 1100..9999 -> {
                val num = roundOffDecimal(item / 1000.0)
                (num + "K")
            }

            in 10_000..999_999 -> {
                ((item / 1000).toString() + "K")
            }

            in 1_000_000..1_000_000_000 -> {
                val num = roundOffDecimal(item / 1_000_000.0)
                (num + "M")
            }

            else -> item.toString()
        }
    }

    private fun roundOffDecimal(number: Double): String {
        val decimalFormat = DecimalFormat("#.#")
        decimalFormat.roundingMode = RoundingMode.DOWN
        return decimalFormat.format(number)
    }

}

