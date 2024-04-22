package org.example

    import com.google.gson.Gson
    import com.google.gson.reflect.TypeToken
    import kotlinx.coroutines.*
    import okhttp3.*
    import okhttp3.logging.HttpLoggingInterceptor
    import org.example.dto.*
    import java.io.IOException
    import java.util.concurrent.TimeUnit
    import kotlin.coroutines.EmptyCoroutineContext
    import kotlin.coroutines.resume
    import kotlin.coroutines.resumeWithException
    import kotlin.coroutines.suspendCoroutine
    
    private val gson = Gson()
    private const val BASE_URL = "http://127.0.0.1:9999"
    private val client = OkHttpClient.Builder()
        .addInterceptor(HttpLoggingInterceptor(::println).apply {
            level = HttpLoggingInterceptor.Level.BODY
        })
        .connectTimeout(30, TimeUnit.SECONDS)
        .build()
    
    fun main() {
        with(CoroutineScope(EmptyCoroutineContext)) {
            launch {
                try {
                    val posts = getPosts(client)
                        .map { post ->
                            async {
                                val authorPost = PostWithAuthor(post, getAuthor(client, post.authorId))
                                val comments = getComments(client, post.id).map { comment ->
                                    CommentsWithAuthors(comment, getAuthor(client, comment.authorId))
                                }

                            PostWithComments(authorPost, comments)
                        }
                    }.awaitAll()
                println(posts)
            } catch (e: Exception) {
                e.printStackTrace()
            }
        }
    }
    Thread.sleep(30_000L)
    }
    
    suspend fun OkHttpClient.apiCall(url: String): Response {
        return suspendCoroutine { continuation ->
            Request.Builder()
                .url(url)
                .build()
                .let(::newCall)
                .enqueue(object : Callback {
                    override fun onResponse(call: Call, response: Response) {
                        continuation.resume(response)
                    }

                override fun onFailure(call: Call, e: IOException) {
                    continuation.resumeWithException(e)
                }
            })
    }
    }

    suspend fun <T> makeRequest(url: String, client: OkHttpClient, typeToken: TypeToken<T>): T =
        withContext(Dispatchers.IO) {
            client.apiCall(url)
                .let { response ->
                    if (!response.isSuccessful) {
                        response.close()
                        throw RuntimeException(response.message)
                    }
                    val body = response.body ?: throw RuntimeException("response body is null")
                    gson.fromJson(body.string(), typeToken.type)
                }
        }

    suspend fun getPosts(client: OkHttpClient): List<Post> =
        makeRequest("$BASE_URL/api/slow/posts", client, object : TypeToken<List<Post>>() {})
    
    suspend fun getComments(client: OkHttpClient, id: Long): List<Comment> =
        makeRequest("$BASE_URL/api/slow/posts/$id/comments", client, object : TypeToken<List<Comment>>() {})
    
    suspend fun getAuthor(client: OkHttpClient, id: Long): Author =
        makeRequest("$BASE_URL/api/slow/authors/$id", client, object : TypeToken<Author>() {})

data class Author (

                val id: Long,
                val name: String,
                val avatar: String,
            )
data class Attachment(

    val url: String,
    val description: String,
    val type: AttachmentType,
    )

    enum class AttachmentType


data class PostWithComments(

    val post: PostWithAuthor,
    val comments: List<CommentsWithAuthors>,
    ) 

data class PostWithAuthor(

    val post: Post,
    val author: Author,
    )
data class CommentsWithAuthors(

        val comment: Comment,
        val author: Author
    )    
data class PostWithAuthorAndComment (

    val id: Long,
    val author: Author,
    val content: String,
    val published: Long,
    val likedByMe: Boolean,
    val likes: Int = 0,
    var attachment: Attachment? = null,
    val comments: List<CommentWithAuthor>,
    )
    
    data class CommentWithAuthor(
        val id: Long,
        val postId: Long,
        val author: Author,
        val content: String,
        val published: Long,
        val likedByMe: Boolean,
        val likes: Int = 0,
    )
  data class Comment(
  
    val id: Long,
    val postId: Long,
    val authorId: Long,
    val content: String,
    val published: Long,
    val likedByMe: Boolean,
    val likes: Int = 0,
    )  
data class Post(

    val id: Long,
    val authorId: Long,
    val content: String,
    val published: Long,
    val likedByMe: Boolean,
    val likes: Int = 0,
    var attachment: Attachment? = null,
    )    
