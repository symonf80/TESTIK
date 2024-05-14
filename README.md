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
            if (!post.attachment?.url.isNullOrBlank()) {
                loadImage.visibility = View.VISIBLE
            } else loadImage.visibility = View.GONE

            val urlAttachment = "${BuildConfig.BASE_URL}/media/${post.attachment?.url}"
            Glide.with(loadImage)
                .load(urlAttachment)
                .timeout(10_000)
                .into(loadImage)


                __________________________

                 <ImageView
            android:id="@+id/loadImage"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:visibility="gone"
            app:layout_constraintTop_toBottomOf="@+id/content"
            app:layout_constraintBottom_toTopOf="@+id/footer"/>
