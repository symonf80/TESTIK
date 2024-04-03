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
