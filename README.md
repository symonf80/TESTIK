

class AppActivity2 : AppCompatActivity(R.layout.activity_app) {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        intent?.let {
            if (it.action != Intent.ACTION_SEND) {
                return@let
            }
            val text = it.getStringExtra(Intent.EXTRA_TEXT)
            if (text?.isNotBlank() != true) {
                return@let
            }
            intent.removeExtra(Intent.EXTRA_TEXT)
            findNavController(R.id.nav_host_fragment).navigate(
                R.id.action_feedFragment_to_newPostFragment,
                Bundle().apply {
                    textArg = text
                }
            )
        }
    }
    }

    package ru.netology.nmedia.view



class FeedFragment : Fragment() {

     private val viewModel: PostViewModel by viewModels(
        ownerProducer = ::requireParentFragment
    )
    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        val binding = FragmentFeedBinding.inflate(
            inflater, container, false
        )

   


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
                findNavController().navigate(
                    R.id.action_feedFragment_to_newPostFragment,
                    Bundle().apply { textArg = post.content })
            }

            override fun onPlay(post: Post) {
                val intent = Intent(Intent.ACTION_VIEW, Uri.parse(post.video))
                val viewIntent = Intent.createChooser(intent, R.string.external_link.toString())
                startActivity(viewIntent)
            }


        })
        binding.list.adapter = adapter
        viewModel.data.observe(viewLifecycleOwner) { posts ->
            val newPost = adapter.currentList.size < posts.size && adapter.currentList.size > 0
            adapter.submitList(posts) {
                if (newPost) {
                    binding.list.smoothScrollToPosition(0)
                }
            }
        }

        viewModel.edited.observe(viewLifecycleOwner) { post ->
            if (post.id != 0L) {
              
            }
        }
        binding.add.setOnClickListener {
           findNavController().navigate(R.id.action_feedFragment_to_newPostFragment)

        }

        return binding.root
    }
    }



object StringArg : ReadWriteProperty<Bundle, String?> {//переместить в Utils

    override fun getValue(thisRef: Bundle, property: KProperty<*>): String? =
        thisRef.getString(property.name)


    override fun setValue(thisRef: Bundle, property: KProperty<*>, value: String?) {
        thisRef.putString(property.name, value)
    }

    }
    ///
    class NewPostFragment : Fragment() {

          companion object {
          var Bundle.textArg: String? by StringArg

    }

    private val viewModel: PostViewModel by viewModels(
        ownerProducer = ::requireParentFragment
    )

    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        val binding = FragmentNewPostBinding.inflate(
            inflater,
            container,
            false
        )

        arguments?.textArg?.let(binding.edit::setText)


        binding.ok.setOnClickListener {
            viewModel.changeContentAndSave(binding.edit.text.toString())
            AndroidUtils.hideKeyboard(requireView())
            findNavController().navigateUp()
        }


       binding.bottomAppBar.setOnMenuItemClickListener { menuItem ->
            when (menuItem.itemId) {
                cancel -> {
                    viewModel.changeContentAndSave(binding.edit.text.toString())

                    findNavController().navigateUp()
                }

                else -> {
                    findNavController().navigateUp()
                }
            }
        }
        return binding.root

    }


    }


<?xml version="1.0" encoding="utf-8"?>

    <androidx.fragment.app.FragmentContainerView xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/nav_host_fragment"
    android:name="androidx.navigation.fragment.NavHostFragment"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:defaultNavHost="true"
    app:navGraph="@navigation/nav_main"
    tools:context=".view.AppActivity2" />
----------------------------------------------------------------------
     <activity
            android:name=".view.AppActivity2"
            android:exported="true"
            android:windowSoftInputMode="adjustResize">
            <nav-graph android:value="@navigation/nav_main" />

            <intent-filter>
                <action android:name="android.intent.action.SEND" />
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>

      </activity>


<?xml version="1.0" encoding="utf-8"?>

    <FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/nav_fragment"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".BlankFragment"
    tools:showIn="@layout/card_post" />

     

    class BlankFragment : Fragment() {
    
    val service = Service()

    companion object {
        var Bundle.idArg: String? by StringArg

    }

    private val viewModel: PostViewModel by viewModels(
        ownerProducer = ::requireParentFragment
    )

    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        val binding = FragmentBlankBinding.inflate(
            inflater, container, false
        )


        val postId = arguments?.idArg ?: -1
        viewModel.data.observe(viewLifecycleOwner) { posts ->
            val post = posts.find { it.id == postId } ?: return@observe
            with(binding) {
                author.text = post.author
                published.text = post.published
                content.text = post.content
                likes.text = service.counter(post.likes)
                share.text = service.counter(post.repost)
                tvViews.text = service.counter(post.views)
                likes.isChecked = post.likedByMe
                if (post.video?.isNotEmpty() == true) {
                    play.visibility = View.VISIBLE
                    video.visibility = View.VISIBLE
                } else {
                    play.visibility = View.GONE
                    video.visibility = View.GONE
                }

                likes.setOnClickListener {


                }
                share.setOnClickListener {

                }
                play.setOnClickListener {

                }
                video.setOnClickListener {

                }


                menu.setOnClickListener {
                    PopupMenu(it.context, it).apply {
                        inflate(R.menu.options_post)
                        setOnMenuItemClickListener { item ->
                            when (item.itemId) {
                                R.id.remove -> {
                                    viewModel.removeById(post.id)
                                    findNavController().navigateUp()
                                    true
                                }

                                R.id.edit -> {
                                    viewModel.edit(post)
                                    findNavController().navigateUp()
                                    findNavController().navigate(
                                        R.id.editPostFragment,
                                        Bundle().apply {
                                            textArg = content.text.toString()
                                        })
                                    true
                                }

                                else -> false
                            }
                        }
                    }.show()
                }
            }
        }

        return binding.root
    }

    }

