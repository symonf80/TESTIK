import java.util.Objects

data class Post(

    val id: Int = 1,
    val ownerId: Int = 2,
    val fromId: Int = 3,
    val date: Int = 4,
    val text: String = "text",
    val comments: Comments? = Comments(),
    val copyright: String = "",
    val likes: Likes? = Likes(),
    val canPin: Boolean = true,
    val isFavorite: Boolean = true
) 
{
   
    data class Comments(
    
        val count: Int = 1,
        val canPost: Boolean = true,
        val groupsCanPost: Boolean = false,
        val canClose: Boolean = true,
        val canOpen: Boolean = false
    )

    data class Likes(
    
        val count: Int = 1,
        val userLikes: Boolean = true,
        val canLike: Boolean = true,
        val canPublish: Boolean = true
    )
}


class WallService {

    private var postId: Int = 1;
    var posts = emptyArray<Post>()

    fun add(post: Post): Post {
        posts += post.copy(id = postId)
        postId++
        return posts.last()
    }

    fun update(post: Post): Boolean {
        for ((index, item) in posts.withIndex()) {
            if (item.id == post.id) {
                posts[index] = post.copy(ownerId = item.ownerId, date = item.date)
                return true
            }
        }
        return false
    }
}

fun main() {

    val post = Post()
    val wall = WallService()
    val post1 = Post(fromId = 8, text = "55")
    wall.add(post)
    wall.add(post1)
    println(wall.add(post1))

}


import org.junit.Test

import org.junit.Assert.*

class WallServiceTest {

    @Test
    fun add() {
        val data = WallService()
        data.add(Post(0, 2, 3, 4, "test", Post.Comments(), "4", Post.Likes(), true, true))
        assertNotEquals(0, data)
    }

    @Test
    fun updateTrue() {
        val service = WallService()

        service.add(Post(1, 2, 3, 4, "test", Post.Comments(), "4", Post.Likes(), true, true))
        service.add(Post(2, 3, 4, 5, "test2", Post.Comments(), "4", Post.Likes(), true, true))
        service.add(Post(3, 4, 5, 6, "test3", Post.Comments(), "4", Post.Likes(), true, true))
        val update = Post(3, 2, 3, 4, "test", Post.Comments(), "4", Post.Likes(), true, true)
        val result = service.update(update)
        assertTrue(result)
    }

    @Test
    fun updateFalse() {
        val service = WallService()
        service.add(Post(1, 2, 3, 4, "test", Post.Comments(), "4", Post.Likes(), true, true))
        service.add(Post(2, 3, 4, 5, "test2", Post.Comments(), "4", Post.Likes(), true, true))
        service.add(Post(3, 4, 5, 6, "test3", Post.Comments(), "4", Post.Likes(), true, true))

        val update = Post(4, 2, 3, 4, "test", Post.Comments(), "4", Post.Likes(), true, true)
        val result = service.update(update)
        assertFalse(result)

    }
}



