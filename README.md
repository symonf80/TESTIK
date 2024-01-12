class MainActivity : AppCompatActivity() {

    private lateinit var binding: ActivityMainBinding
    private var count: Int = 0
    private var like = false
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
        binding.likes.setOnClickListener {
            if (!like) {
                binding.likes.setImageResource(R.drawable.baseline_favorite_24)
                like = true
                count++
                binding.tvLikes.text = count.toString()
            } else {
                binding.likes.setImageResource(R.drawable.baseline_favorite_border_24)
                like = false
                count--
                binding.tvLikes.text = count.toString()
            }
        }
    }

}
