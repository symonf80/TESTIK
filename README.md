
data class Audio(

    val id: Int = 1,
    val ownerId: Int = 2,
    val artist: String = "artist",
    val title: String = "title",
    val duration: Int = 3,
    val url: String = "url",
    val lyricsId: Int = 1,
    val albumId: Int = 1,
    val genreId: Int = 1,
    val date: Int = 1,
    val noSearch: Boolean = true,
    val isHd: Boolean = true
)

data class Document(

    val id: Int = 1,
    val ownerId: Int = 1,
    val title: String = "title",
    val size: Int = 1,
    val ext: String = "ext",
    val url: String = "url",
    val date: Int = 1,
    val type: Int = 1,
    val preview: Preview = Preview()
) {

    data class Preview(
        val photo: Array<Photo> = emptyArray(),
        val graffiti: Graffiti = Graffiti(),
        val audioMessage: AudioMessage = AudioMessage(),
    ) {

        data class Photo(
            val sizes: Int = 1
        )


        data class Graffiti(
            val src: String = "src",
            val width: Int = 10,
            val height: Int = 5
        )

        data class AudioMessage(
            val duration: Int = 100,
            val waveform: Array<Int> = emptyArray<Int>(),
            val linkOgg: String = "linkOgg",
            val linkMp3: String = "mp3"
        )
    }
}

data class Link(

    val url: String = "url",
    val title: String = "title",
    val caption: String = "caption",
    val description: String = "description",
    val photo: Photo = Photo(),
    val product: Product = Product(),
    val button: Button = Button(),
    val previewPage: String = "previewPage",
    val previewUrl: String = "previewUrl"
) {

    data class Product(
        val price: String = "price"
    )

    class Button()
}

data class Note(

    val id: Int = 1,
    val ownerId: Int = 1,
    val title: String = "title",
    val text: String = "text",
    val date: Int = 1,
    val comments: Int = 1,
    val readComments: Int = 1,
    val viewUrl: String = "viewUrl"
)

data class Page(

    val id: Int = 1,
    val groupId: Int = 1,
    val creatorId: Int = 1,
    val title: String = "title",
    val currentUserCanEdit: Boolean = true,
    val currentUserCanEditAccess: Boolean = true,
    val whoCanView: Int = 1,
    val whoCanEdit: Int = 1,
    val edited: Int = 1,
    val created: Int = 1,
    val editorId: Int = 1,
    val views: Int = 1,
    val parent: String = "parent",
    val parent2: String = "parent2",
    val source: String = "source",
    val html: String = "html",
    val viewUrl: String = "viewUrl"
)

data class Photo(

    val id: Int = 1,
    val albumId: Int = 1,
    val ownerId: Int = 1,
    val userId: Int = 1,
    val text: String = "text",
    val date: Int = 1,
    val sizes: Array<Sizes> = emptyArray(),
    val width: Int = 1,
    val height: Int = 1

) {
    data class Sizes(
    
        val type: String = "type",
        val url: String = "url",
        val width: Int = 100,
        val height: Int = 100
    )
}

data class Poll(

    val id: Int = 1,
    val ownerId: Int = 1,
    val created: Int = 1,
    val question: String = "question",
    val votes: Int = 1,

    )


data class Post(

    val id: Int = 1,
    val ownerId: Int = 1,
    val fromId: Int = 1,
    val createdBy: Int = 1,
    val date: Int = 1,
    val text: String = "text",
    val replyOwnerId: Int = 1,
    val replyPostId: Int = 1,
    val friendsOnly: Boolean = true,
    val comments: Comments = Comments(),
    val copyright: String = "copyright",
    val likes: Likes = Likes(),
    val reposts: Reposts = Reposts(),
    val views: Views = Views(),
    val postType: String = "postType",
    val postSource: PostSource = PostSource(),
    val attachments: Array<Attachment> = emptyArray(),
    val geo: Geo = Geo(),
    val signerId: Int = 1,
    val copyHistory: Array<CopyHistory> = emptyArray(),
    val canPin: Boolean = true,
    val canDelete: Boolean = true,
    val canEdit: Boolean = true,
    val isPinned: Boolean = true,
    val markedAsAds: Boolean = true,
    val isFavorite: Boolean = true,
    val postponedId: Int = 1

) {
    data class Comments(
    
        val count: Int = 10,
        val canPost: Boolean = true,
        val groupsCanPost: Boolean = true,
        val canClose: Boolean = true,
        val canOpen: Boolean = true
    )

    data class Likes(
    
        val count: Int = 1,
        val userLikes: Boolean = true,
        val canLike: Boolean = true,
        val canPublish: Boolean = true
    )

    data class Reposts(
    
        val count: Int = 2,
        val userReposted: Boolean = true
    )

    data class Views(
        val count: Int = 1
    )


    data class Geo(
    
        val type: String = "type",
        val coordinates: String = "coordinates",
        val place: Place = Place()
    )

    data class Place (
        val someData: Int=1
    )

   data class CopyHistory (
   
        val someData: Int=1
   )
}

data class PostSource(

    val type: String = "type",
    val platform: String = "platform",
    val data: String = "data",
    val url: String = "url"
)

data class Sticker(

    val productId: Int = 1,
    val stickerId: Int = 1,
    val images: Array<Images> = emptyArray(),
    val imagesWithBackground: Array<ImagesWithBackground> = emptyArray(),
) {
    data class Images(
    
        val url: String = "url",
        val width: Int = 100,
        val height: Int = 50
    )


    data class ImagesWithBackground(
    
        val url: String = "url",
        val width: Int = 100,
        val height: Int = 50
    )


}

data class Video(

    val id: Int = 1,
    val ownerId: Int = 1,
    val title: String = "title",
    val description: String = "description",
    val duration: String = "long",
    val photo130: String = "130",
    val photo320: String = "320",
    val photo640: String = "640",
    val photo800: String = "800",
    val photo1280: String = "1280",
    val firstFrame130: String = "f130",
    val firstFrame320: String = "f320",
    val firstFrame640: String = "f640",
    val firstFrame800: String = "f800",
    val firstFrame1280: String = "f1280",
    val date: Int = 1,
    val addingDate: Int = 1,
    val views: Int = 1,
    val comments: Int = 1,
    val player: String = "player",
    val platform: String = "platform",
    val canEdit: Boolean = true,
    val canAdd: Boolean = true,
    val isPrivate: Boolean = true,
    val accessKey: String = "key",
    val processing: Boolean = true,
    val live: Boolean = true,
    val upcoming: Boolean = true,
    val isFavorite: Boolean = true

)


interface Attachment {
    val type: String
}

class AttachmentAudio(val audio: Audio) : Attachment {

    override val type: String = "audio"
    override fun toString(): String {
        return "\n $audio"
    }

}

class AttachmentPhoto(val photo: Photo) : Attachment {

    override val type: String = "photo"
    override fun toString(): String {
        return "\n $photo"
    }
}

class AttachmentVideo(val video: Video) : Attachment {

    override val type: String = "video"
    override fun toString(): String {
        return "\n $video"
    }
}

class AttachmentDoc(val doc: Document) : Attachment {

    override val type: String = "doc"
    override fun toString(): String {
        return "\n $doc"
    }
}

class AttachmentLink(val link: Link) : Attachment {

    override val type: String = "link"
    override fun toString(): String {
        return "\n $link"
    }
}

class AttachmentNote(val note: Note) : Attachment {

    override val type: String = "note"
    override fun toString(): String {
        return "\n $note"
    }
}

class AttachmentPoll(val poll: Poll) : Attachment {

    override val type: String = "poll"
    override fun toString(): String {
        return "\n $poll"
    }
}

class AttachmentPage(val page: Page) : Attachment {

    override val type: String = "page"
    override fun toString(): String {
        return "\n $page"
    }
}

class AttachmentSticker(val sticker: Sticker) : Attachment {

    override val type: String = "sticker"
    override fun toString(): String {
        return "\n $sticker"
    }
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
    wall.add(post)
    println(wall.posts.last())
}


import org.junit.Test

import org.junit.Assert.*

class WallServiceTest {

    @Test
    fun add() {
    
        val data = WallService().add(Post()).id
        assertNotEquals(0, data)
    }
    @Test
    fun updateTrue() {
    
        val service = WallService()
        service.add(Post(1,1,1,1,1,"text",1,1,true,Post.Comments()))
        service.add(Post(2, 3, 4, 5, 1,"test2",1,1,true, Post.Comments()))
        service.add(Post(3, 3, 4, 5, 1,"test2",1,1,true, Post.Comments()))
        val update = Post(3, 3, 4, 5, 1,"test2",1,1,true, Post.Comments())
        val result = service.update(update)
        assertTrue(result)
    }

    @Test
    fun updateFalse() {
    
        val service = WallService()
        service.add(Post(1,1,1,1,1,"text",1,1,true,Post.Comments()))
        service.add(Post(2, 3, 4, 5, 1,"test2",1,1,true, Post.Comments()))
        service.add(Post(3, 3, 4, 5, 1,"test2",1,1,true, Post.Comments()))
        val update = Post(5,1,1,1,1,"text",1,1,true,Post.Comments())
        val result = service.update(update)
        assertFalse(result)

    }
}
