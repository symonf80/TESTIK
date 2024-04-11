package ru.netology.nmedia.dto

    data class Post(
    val id: Long,
    val authorAvatar: String,
    val author: String,
    val content: String,
    val published: String,
    val likedByMe: Boolean,
    val attachment: Attachment? = Attachment(),
    val likes: Int = 0,
    ) {
    data class Attachment(
        val url: String="",
        val description:String="",
        val type: String=""

    )
    }

   package ru.netology.nmedia.adapter
   
     class PostViewHolder(
    private val binding: CardPostBinding,
    private val onInteractionListener: OnInteractionListener,
    ) : RecyclerView.ViewHolder(binding.root) {

    fun bind(post: Post) {
        binding.apply {
            author.text = post.author
            published.text = post.published
            content.text = post.content
            // в адаптере
            like.isChecked = post.likedByMe
            like.text = "${post.likes}"

            if (!post.attachment?.url.isNullOrEmpty()) {
                attachment.visibility = View.VISIBLE

            } else {
                attachment.visibility = View.GONE

            }

            val url = "http://10.0.2.2:9999/avatars/${post.authorAvatar}"
            Glide.with(avatar)
                .load(url)
                .apply(RequestOptions.circleCropTransform())
                .placeholder(R.drawable.baseline_downloading_24)
                .error(R.drawable.baseline_error_24)
                .timeout(10_000)
                .into(avatar)

            val urlAttach = "http://10.0.2.2:9999/images/${post.attachment?.url}"
            Glide.with(attachment)
                .load(urlAttach)
                .timeout(10_000)
                .into(attachment)
                ...
  package ru.netology.nmedia.viewmodel

          import android.app.Application
        import androidx.lifecycle.*
        import ru.netology.nmedia.dto.Post
        import ru.netology.nmedia.model.FeedModel
        import ru.netology.nmedia.repository.*
        import ru.netology.nmedia.util.SingleLiveEvent

        private val empty = Post(
    id = 0,
    content = "",
    authorAvatar = "",
    author = "",
    likedByMe = false,
    likes = 0,
    attachment = null,
    published = ""
        )

   <ImageView
           
            android:id="@+id/attachment"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginStart="0dp"
            android:layout_marginTop="0dp"
            android:layout_marginEnd="0dp"
            android:layout_marginBottom="0dp"
            android:importantForAccessibility="no"
            android:visibility="gone"
            app:layout_constraintBottom_toBottomOf="@+id/footer"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toBottomOf="@id/content" />
