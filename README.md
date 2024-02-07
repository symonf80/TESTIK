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
    
    <?xml version="1.0" encoding="utf-8"?>
    <androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".view.MainActivity">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/list"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layoutManager="androidx.recyclerview.widget.LinearLayoutManager"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        tools:listitem="@layout/card_post" />

    <com.google.android.material.floatingactionbutton.FloatingActionButton
        android:id="@+id/add"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="16dp"
        android:importantForAccessibility="no"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:srcCompat="@drawable/baseline_add_24" />

    <androidx.constraintlayout.widget.Group
        android:id="@+id/group"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginBottom="24dp"
        android:background="@color/white"
        android:visibility="gone"
        app:constraint_referenced_ids="create,message,netol,close"
        app:layout_constraintBottom_toBottomOf="parent" />

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

     </androidx.constraintlayout.widget.ConstraintLayout>







