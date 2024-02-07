class NewPostActivity : AppCompatActivity() {

       override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val binding = ActivityNewPostBinding.inflate(layoutInflater)
        setContentView(binding.root)

        binding.ok.setOnClickListener {
            val text = binding.edit.text.toString()
            if (text.isNotBlank()) {
                setResult(RESULT_OK, Intent().apply { putExtra(Intent.EXTRA_TEXT, text) })
            } else {
                setResult(RESULT_CANCELED)
            }
            finish()
        }
    }
    }

    object NewPostContract : ActivityResultContract<Unit, String?>() {
    override fun createIntent(context: Context, input: Unit) =
        Intent(context, NewPostActivity::class.java)


    override fun parseResult(resultCode: Int, intent: Intent?) =
        intent?.getStringExtra(Intent.EXTRA_TEXT)

    }

   <?xml version="1.0" encoding="utf-8"?>
    <androidx.coordinatorlayout.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"

    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <EditText
        android:id="@+id/edit"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="@android:color/transparent"
        android:gravity="top"
        android:hint="Post text"
        android:importantForAutofill="no"
        android:inputType="textMultiLine"
        android:padding="16dp"

        />

    <com.google.android.material.bottomappbar.BottomAppBar
        android:id="@+id/bottomAppBar"
        style="@style/Widget.Material3.BottomAppBar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom"

        />

    <com.google.android.material.floatingactionbutton.FloatingActionButton
        android:id="@+id/ok"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:importantForAccessibility="no"
        app:layout_anchor="@id/bottomAppBar"
        app:srcCompat="@drawable/baseline_add_24" />


    </androidx.coordinatorlayout.widget.CoordinatorLayout>




class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
        val viewModel: PostViewModel by viewModels()

        val newPostLauncher = registerForActivityResult(NewPostContract) {result->
            result?:return@registerForActivityResult
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
     
        binding.add.setOnClickListener {
            newPostLauncher.launch()
   

        }
    }
    }
 <style name="Widget.AppTheme.PlayButton" parent="Widget.Material3.Button.IconButton">      
                <item name="iconTint">@color/white</item>
        <item name="icon">@drawable/baseline_arrow_right_24</item>
        <item name="backgroundTint">@android:color/transparent</item>
       <item name="iconSize">48dp</item>
    </style>
    
