data class Comment(

    var noteId: Int =1,
    val date: Int = 3,
    var message: String = "text",
    var isDeleted: Boolean = false
)

class NoteService {

    var notes = mutableListOf<Note>()
    private var noteId: Int = 0

    fun add(note: Note) {    
        if (notes.isEmpty()) {
            noteId = 1
        } else {
            note.id = notes.last().id + 1
        }
        notes.add(note)
    }

    fun createComment(id: Int, comment: Comment) {
        for (note in notes) {
            if (id == note.id) {
                if (note.comments.isEmpty()) {
                    comment.noteId = 1
                } else {
                    comment.noteId = note.comments.last().noteId + 1
                }
                note.comments.add(comment)
            }
        }
    }

    fun delete(id: Int) {
        val iterator = notes.iterator()
        while (iterator.hasNext()) {
            if (id == iterator.next().id) {
                iterator.remove()
            }
        }
    }

    fun deleteComment(noteId: Int, commentId: Int) {
        for (note in notes) {
            if (noteId == note.id) {
                for (comment in note.comments) {
                    if (comment.noteId == commentId && !comment.isDeleted) {
                        comment.isDeleted = true

                    }
                }
            }
        }
    }

    fun edit(id: Int, title: String, text: String) {
        for (note in notes) {
            if (id == note.id) {
                note.title = title
                note.text = text
            }
        }
    }

    fun editComment(noteId: Int, commentId: Int, message: String) {
        for (note in notes) {
            if (noteId == note.id) {
                for (comment in note.comments) {
                    if (comment.noteId == commentId && !comment.isDeleted) {
                        comment.message = message
                    }
                }
            }
        }
    }

    fun get(): List<Note>? {
        return notes.ifEmpty {
            null
        }
    }

    fun getById(id: Int): Note? {
        for (note in notes) {
            if (id == note.id) {
                return note
            }
        }
        return null
    }

    fun getComment(noteId: Int): List<Comment>? {
        val comments = mutableListOf<Comment>()
        for (note in notes) {
            if (note.id == noteId) {
                for (comment in note.comments) {
                    if (!comment.isDeleted) {
                        comments.add(comment)
                    }
                }
                return comments
            }
        }
        return null
    }

    fun restoreComment(noteId: Int, commentId: Int) {
        for (note in notes) {
            if (noteId == note.id) {
                for (comment in note.comments) {
                    if (comment.noteId == commentId && comment.isDeleted) {
                        comment.isDeleted = false
                    }
                }
            }
        }
    }
}
data class Note(

    var id: Int = 1,
    var title: String = "title",
    var text: String = "text",
    val date: Int = 1,
    val comments: MutableList<Comment> = mutableListOf(),
    val readComments: Int = 1,
    val viewUrl: String = "viewUrl"
)

fun main() {

    val service = NoteService()
    val note = Note()
    val note1 = Note(id = 2)
    val com = Comment(message = "new comment")
    val com1 = Comment(message = "second comment")
    service.add(note)
    service.add(note1)
    service.createComment(1, com)
    service.createComment(2, com1)
    println(service.notes[0])
    println(service.notes[1])
    
//    service.deleteComment(1,1)

//    println(service.notes[0])

//    println(service.notes[1])

    service.editComment(2,1,"edit message")
    println(service.getById(1))
    println(service.notes[0])
    println(service.notes[1])
//    println("\nВывод списка заметок")

//    println(service.get())

//    println("\nВосстановление комментария:")

//    service.restoreComment(1, 1)

//    println(service.notes[0])

}

import org.junit.Assert.*
import org.junit.Test

class NoteServiceTest {

    @Test
    fun addNoteIfListEmpty() {
        val service = NoteService()
        val comment = Comment(message = "First Comment")
        val note = Note(
            title = "First Note",
            text = "First Text",
            comments = mutableListOf(comment)
        )
        service.add(note)
        val result = note.id
        assertEquals(1, result)
    }

    @Test
    fun addNoteIfListNotEmpty() {
        val service = NoteService()
        val comment = Comment(message = "First Comment")
        val noteFirst = Note(
            title = "First Title",
            text = "First Text",
            comments = mutableListOf(comment)
        )

        val noteSecond = Note(
            title = "Second Title",
            text = "Second Text",
            comments = mutableListOf(comment)
        )
        service.add(noteFirst)
        service.add(noteSecond)
        val result = noteSecond.id
        assertEquals(2, result)
    }

    @Test
    fun deleteNoteWithExistId() {
        val service = NoteService()
        val comment = Comment(message = "First Comment")
        val note = Note(
            title = "First Title",
            text = "First Text",
            comments = mutableListOf(comment)
        )
        service.add(note)
        service.delete(1)
        val result = service.notes.size
        assertEquals(0, result)
    }

    @Test
    fun deleteNoteWithNotExistId() {
        val service = NoteService()
        val comment = Comment(message = "First Comment")
        val note = Note(
            title = "First Title",
            text = "First Text",
            comments = mutableListOf(comment)
        )
        service.add(note)
        service.delete(2)
        val result = service.notes.size
        assertEquals(1, result)
    }

    @Test
    fun editNoteWithExistId() {
        val service = NoteService()
        val comment = Comment(message = "First Comment")
        val title = "First Title"
        val text = "First Text"
        val note = Note(
            title = "First Title",
            text = "First Text",
            comments = mutableListOf(comment)
        )
        service.add(note)
        service.edit(1, title, text)
        val resultTitle = note.title
        val resultText = note.text
        assertEquals(title, resultTitle)
        assertEquals(text, resultText)
    }

    @Test
    fun editNoteWithNotExistId() {
        val service = NoteService()
        val comment = Comment(message = "First Comment")
        val title = "First Title"
        val text = "First Text"
        val note = Note(
            title = "First Title",
            text = "First Text",
            comments = mutableListOf(comment)
        )
        service.add(note)
        service.edit(2, title, text)
        val resultTitle = note.title
        val resultText = note.text
        assertEquals("First Title", resultTitle)
        assertEquals("First Text", resultText)
    }

    @Test
    fun getAllNotes() {
        val service = NoteService()
        val comment = Comment(message = "First Comment")
        val noteFirst = Note(
            title = "First Title",
            text = "First Text",
            comments = mutableListOf(comment)
        )

        val noteSecond = Note(
            title = "Second Title",
            text = "Second Text",
            comments = mutableListOf(comment)
        )

        val noteThird = Note(
            title = "Third Title",
            text = "Third Text",
            comments = mutableListOf(comment)
        )
        service.add(noteFirst)
        service.add(noteSecond)
        service.add(noteThird)
        val result = service.get()?.size
        assertEquals(3, result)
    }

    @Test
    fun getNoteIfExistId() {
        val service = NoteService()
        val comment = Comment(message = "First Comment")
        val noteFirst = Note(
            title = "First Title",
            text = "First Text",
            comments = mutableListOf(comment)
        )

        val noteSecond = Note(
            title = "Second Title",
            text = "Second Text",
            comments = mutableListOf(comment)
        )
        service.add(noteFirst)
        service.add(noteSecond)
        val result = service.getById(2)
        assertEquals(2, result?.id)
    }

    @Test
    fun getNoteIfNotExistId() {
        val service = NoteService()
        val comment = Comment(message = "First Comment")
        val noteFirst = Note(
            title = "First Title",
            text = "First Text",
            comments = mutableListOf(comment)
        )

        val noteSecond = Note(
            title = "Second Title",
            text = "Second Text",
            comments = mutableListOf(comment)
        )
        service.add(noteFirst)
        service.add(noteSecond)
        val result = service.getById(3)
        assertEquals(null, result?.id)
    }

    @Test
    fun addCommentIfCommentsListIsEmpty() {
        val service = NoteService()
        val comment = Comment(message = "First Comment")
        val note = Note(
            title = "First Title",
            text = "First Text",
            comments = mutableListOf()
        )
        service.add(note)
        service.createComment(1, comment)
        val result = note.comments.size
        assertEquals(1, result)
    }

    @Test
    fun addCommentIfCommentsListIsNotEmpty() {
        val service = NoteService()
        val commentFirst = Comment(message = "First Comment")
        val commentSecond = Comment(message = "Second Comment")
        val note = Note(
            title = "First Title",
            text = "First Text",
            comments = mutableListOf()
        )

        service.add(note)
        service.createComment(1, commentFirst)
        service.createComment(1, commentSecond)
        val result = commentSecond.noteId
        assertEquals(2, result)
    }

    @Test
    fun addCommentNotValidNote() {
        val service = NoteService()
        val comment = Comment(message = "First Comment")
        val note = Note(
            title = "First Title",
            text = "First Text",
            comments = mutableListOf()
        )
        service.add(note)
        service.createComment(2312, comment)
        val result = note.comments.size
        assertEquals(0, result)
    }

    @Test
    fun deleteComment() {
        val service = NoteService()
        val commentFirst = Comment(message = "First Comment")
        val commentSecond = Comment(message = "Second Comment")
        val note = Note(
            title = "First Title",
            text = "First Text",
            comments = mutableListOf()
        )
        service.add(note)
        service.createComment(1, commentFirst)
        service.createComment(1, commentSecond)
        service.deleteComment(1, 1)
        val result = commentFirst.isDeleted
        assertTrue(result)
    }

    @Test
    fun deleteCommentIfNotExistNote() {
        val service = NoteService()
        val commentFirst = Comment(message = "First Comment")
        val commentSecond = Comment(message = "Second Comment")
        val note = Note(
            title = "First Title",
            text = "First Text",
            comments = mutableListOf()
        )
        service.add(note)
        service.createComment(1, commentFirst)
        service.createComment(1, commentSecond)
        service.deleteComment(2, 1)
        val result = note.comments.size
        assertEquals(2, result)
    }

    @Test
    fun deleteDeletedComment() {
        val service = NoteService()
        val commentFirst = Comment(message = "First Comment", isDeleted = true)
        val note = Note(
            title = "First Title",
            text = "First Text",
            comments = mutableListOf()
        )
        service.add(note)
        service.createComment(1, commentFirst)
        service.deleteComment(1, 1)
        val resultFirst = note.comments.size
        val resultSecond = commentFirst.isDeleted
        assertEquals(1, resultFirst)
        assertEquals(true, resultSecond)
    }

    @Test
    fun restoreComment() {
        val service = NoteService()
        val comment = Comment(message = "First Comment")
        val note = Note(
            title = "First Title",
            text = "First Text",
            comments = mutableListOf()
        )
        service.add(note)
        service.createComment(1, comment)
        service.deleteComment(1, 1)
        service.restoreComment(1, 1)
        val resultFirst = comment.isDeleted
        val resultSecond = note.comments.size
        assertFalse(resultFirst)
        assertEquals(1, resultSecond)
    }

    @Test
    fun restoreCommentIfNotExistNote() {
        val service = NoteService()
        val comment = Comment(message = "First Comment")
        val note = Note(
            title = "First Title",
            text = "First Text",
            comments = mutableListOf()
        )
        service.add(note)
        service.createComment(1, comment)
        service.deleteComment(1, 1)
        service.restoreComment(2, 1)
        val resultFirst = comment.isDeleted
        val resultSecond = note.comments.size
        assertTrue(resultFirst)
        assertEquals(1, resultSecond)
    }

    @Test
    fun restoreNotValidComment() {
        val service = NoteService()
        val comment = Comment(message = "First Comment", isDeleted = false)
        val note = Note(
            title = "First Title",
            text = "First Text",
            comments = mutableListOf()
        )
        service.add(note)
        service.createComment(1, comment)
        service.restoreComment(1, 2)
        val result = note.comments.size
        assertEquals(1, result)
    }

    @Test
    fun get() {
        val service = NoteService()
        val commentFirst = Comment(message = "First Comment")
        val commentSecond = Comment(message = "Second Comment")
        val commentThird = Comment(message = "Third Comment")
        val note = Note(
            title = "First Title",
            text = "First Text",
            comments = mutableListOf()
        )
        service.add(note)
        service.createComment(1, commentFirst)
        service.createComment(1, commentSecond)
        service.createComment(1, commentThird)
        val result = service.getComment(1)?.size
        assertEquals(3, result)
    }

    @Test
    fun getCommentsNotExistNote() {
        val service = NoteService()
        val commentFirst = Comment(message = "First Comment")
        val commentSecond = Comment(message = "Second Comment")
        val commentThird = Comment(message = "Third Comment")
        val note = Note(
            title = "First Title",
            text = "First Text",
            comments = mutableListOf()
        )
        service.add(note)
        service.createComment(1, commentFirst)
        service.createComment(1, commentSecond)
        service.createComment(1, commentThird)
        val result = service.getComment(2)?.size
        assertEquals(null, result)
    }

    @Test
    fun editComment() {
        val service = NoteService()
        val comment = Comment(message = "First Comment")
        val note = Note(
            title = "First Title",
            text = "First Text",
            comments = mutableListOf()
        )
        service.add(note)
        service.createComment(1, comment)
        service.editComment(1, 1, "New Text")
        val result = comment.message
        assertEquals("New Text", result)
    }

    @Test
    fun editCommentIfNotExistNote() {
        val service = NoteService()
        val comment = Comment(message = "First Comment")
        val note = Note(
            title = "First Title",
            text = "First Text",
            comments = mutableListOf()
        )
        service.add(note)
        service.createComment(1, comment)
        service.editComment(2, 1, "New Text")
        val result = comment.message
        assertEquals("First Comment", result)
    }

    @Test
    fun editDeletedComment() {
        val service = NoteService()
        val comment = Comment(message = "First Comment", isDeleted = true)
        val note = Note(
            title = "First Title",
            text = "First Text",
            comments = mutableListOf()
        )
        service.add(note)
        service.createComment(1, comment)
        service.editComment(1, 1, "New Text")
        val result = comment.message
        assertEquals("First Comment", result)
    }

    @Test
    fun editNotValidComment() {
        val service = NoteService()
        val comment = Comment(message = "First Comment", isDeleted = false)
        val note = Note(
            title = "First Title",
            text = "First Text",
            comments = mutableListOf()
        )
        service.add(note)
        service.createComment(1, comment)
        service.editComment(1, 2, "New Text")
        val result = comment.message
        assertEquals("First Comment", result)
    }
}



