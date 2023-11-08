//data class Post(

//    val id: Int = 1,
//    val audio: Audio = Audio(),
//    val document: Document? = Document(),
//    val link: Link = Link(),
//    val note: Note = Note(),
//    val page: Page = Page(),
//    val photo: Photo = Photo(),
//    val poll: Poll = Poll(),
//    val post: Post? = Post(),
//    val postSource: PostSource = PostSource(),
//    val sticker: Sticker? = Sticker(),
//    val video: Video = Video(),


 //   )

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
            val photo: Photo = Photo,
            val graffiti: Graffiti = Graffiti,
            val audioMessage: AudioMessage = AudioMessage,
        ) {

            object Photo {
                val sizes = Array(1) { 1 }
            }


            object Graffiti {
                val src: String = "src"
                val width: Int = 10
                val height: Int = 5
            }

            object AudioMessage {
                val duration: Int = 100
                val waveform = Array(1) { 100 }
                val linkOgg: String = "linkOgg"
                val linkMp3: String = "mp3"
            }
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
        class Photo {

        }

        class Product {
            val price = "price"
        }

        class Button {

        }
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
        val attachments: Attachments = Attachments(),
        val geo: Geo = Geo(),
        val signerId: Int = 1,
        val copyHistory: CopyHistory = CopyHistory(),
        val canPin: Boolean = true,
        val canDelete: Boolean = true,
        val canEdit: Boolean = true,
        val isPinned: Boolean = true,
        val markedAsAds: Boolean = true,
        val isFavorite: Boolean = true,
        val postponedId: Int = 1
    ) {
       data class Comments (
            val count :Int= 10,
            val canPost :Boolean= true,
            val groupsCanPost :Boolean= true,
            val canClose :Boolean= true,
            val canOpen :Boolean= true
       )

        class Likes {
            val count = 1
            val userLikes = true
            val canLike = true
            val canPublish = true
        }

        class Reposts {
            val count = 2
            val userReposted = true
        }

        class Views {
            val count = 1
        }

        class PostSource {

        }

        class Attachments {
            val array = Array(1) { 1 }
        }

        class Geo {
            val type = "type"
            val coordinates = "coordinates"
            val place = Place()
        }

        class Place {

        }

        class CopyHistory {
            val arr = Array(1) { 2 }
        }
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
        val images: Images = Images(),
        val imagesWithBackground: ImagesWithBackground = ImagesWithBackground()
    ) {
        class Images {
            val arr = Array(1) { 1 }
        }

        class ImagesWithBackground {
            val arr = Array(1) { 1 }
        }
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

class AttachmentAudio(param: Audio) : Attachment {

    override val type: String = "audio"
    val audio = param
    override fun toString(): String {
        return "\n $audio"
    }

}

class AttachmentPhoto(param: Photo) : Attachment {

    override val type: String = "photo"
    val photo = param
    override fun toString(): String {
        return "\n $photo"
    }
}

class AttachmentVideo(param: Video) : Attachment {

    override val type: String = "video"
    val video = param
    override fun toString(): String {
        return "\n $video"
    }
}

class WallService {
    private var postId: Int = 1;
    var posts = emptyArray<Post>()
    fun add(post: Post): Post {
        posts += post
        postId++
        return posts.last()
    }


}

fun main() {
    val post = Post()
    val wall = WallService()
    wall.add(post)
    println(wall.posts.last())
}



