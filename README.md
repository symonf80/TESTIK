<?xml version="1.0" encoding="utf-8"?>
    <FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="#000000"
        tools:context=".activity.OneImageFragment">

    <ImageView
        android:id="@+id/oneImage"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_gravity="center"
        android:importantForAccessibility="no" />

     </FrameLayout>
     _________________________________________________________
     package ru.netology.nmedia.activity

    import android.os.Bundle
    import androidx.fragment.app.Fragment
    import android.view.LayoutInflater
    import android.view.View
    import android.view.ViewGroup
    import androidx.fragment.app.viewModels
    import com.bumptech.glide.Glide
    import ru.netology.nmedia.BuildConfig
    import ru.netology.nmedia.databinding.FragmentOneImageBinding
    import ru.netology.nmedia.util.StringArg
    import ru.netology.nmedia.view.load
    import ru.netology.nmedia.viewmodel.PostViewModel
    
    class OneImageFragment : Fragment() {

    companion object {
        var Bundle.urlArg: String? by StringArg
    }
    private val viewModel: PostViewModel by viewModels(
        ownerProducer = ::requireParentFragment,
    )
    private var fragmentBinding: FragmentOneImageBinding? = null

    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        val binding = FragmentOneImageBinding.inflate(
            inflater,
            container,
            false
        )
        fragmentBinding = binding
        arguments?.urlArg?.let { url->
            with(binding){
                oneImage.load("${BuildConfig.BASE_URL}/media/${url}")
            }
        }

        return binding.root
    }


    }
____________________________________________
    class PostViewHolder(
        private val binding: CardPostBinding,
        private val onInteractionListener: OnInteractionListener,
    ) : RecyclerView.ViewHolder(binding.root) {


    fun bind(post: Post) {
        binding.apply {
            author.text = post.author
            published.text = post.published.toString()
            content.text = post.content
            avatar.loadCircleCrop("${BuildConfig.BASE_URL}/avatars/${post.authorAvatar}")
            like.isChecked = post.likedByMe
            like.text = "${post.likes}"
            when {
                post.attachment != null -> {
                    when (post.attachment.type) {
                        AttachmentType.IMAGE -> {
                            loadImage.visibility = View.VISIBLE
                            loadImage.load("${BuildConfig.BASE_URL}/media/${post.attachment.url}")
                            loadImage.setOnClickListener {
                                loadImage.findNavController()
                                    .navigate(
                                        R.id.action_feedFragment_to_oneImageFragment,
                                        Bundle().apply {
                                            urlArg = post.attachment.url
                                        }
                                    )
                            }
                        }
        
                    }
                }

                else -> {
                    loadImage.visibility = View.GONE
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

            like.setOnClickListener {
                onInteractionListener.onLike(post)
            }

            share.setOnClickListener {
                onInteractionListener.onShare(post)
            }


        }
    }
    }
    
    class PostDiffCallback : DiffUtil.ItemCallback<Post>() {
        override fun areItemsTheSame(oldItem: Post, newItem: Post): Boolean {
            return oldItem.id == newItem.id
        }

    override fun areContentsTheSame(oldItem: Post, newItem: Post): Boolean {
        return oldItem == newItem
    }
    }

    
