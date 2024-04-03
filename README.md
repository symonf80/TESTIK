в репозиторий

    override fun likeById(id: Long) {
 
        val request = Request.Builder()
            .post(RequestBody.create(null, ByteArray(0)))
            .url("${BASE_URL}/api/slow/posts/$id/likes")
            .build()
        client.newCall(request)
            .execute()
            .close()
    }

    override fun unlikePost(id: Long) {
        val request = Request.Builder()
            .delete()
            .url("${BASE_URL}/api/posts/$id/likes")
            .build()
        client.newCall(request)
            .execute()
            .close()
    }


во viewModel

      fun like(id: Long) = thread {
        // Получаем текущий пост
        val currentPosts = _data.value?.posts.orEmpty()
        val currentPost = currentPosts.find { it.id == id }

        currentPost?.let { post ->
            // Оптимистичное обновление UI
            val updatedPost = if (post.likedByMe) {
                post.copy(likes = post.likes - 1, likedByMe = false)
            } else {
                post.copy(likes = post.likes + 1, likedByMe = true)
            }

            // Обновляем список постов
            val updatedPosts = currentPosts.map {
                if (it.id == id) updatedPost else it
            }
            _data.postValue(_data.value?.copy(posts = updatedPosts))

            // Асинхронное обновление на сервере
            try {
                if (post.likedByMe) {
                    repository.unlikePost(id)
                } else {
                    repository.likePost(id)
                }
            } catch (e: IOException) {
                // Если запрос не удался, откатываем изменения в UI
                _data.postValue(_data.value?.copy(posts = currentPosts))
            }
        }
    }
