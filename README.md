class ChatService {

    var chats = mutableMapOf<Int, Chat>()
    private val alert = "Нет сообщений"
    fun addMessage(id: Int, message: Message) {
        chats.getOrPut(id) { Chat() }.messages += message
    }

    fun printChats(): Any {
        return chats.ifEmpty { "Пока пусто" }
    }

    fun deleteChat(id: Int) {
        val chat = chats[id] ?: throw ChatNotFoundException("Чатов нет")
        chat.messages.removeAll { _ -> true }
        chat.messages += Message("Данный чат удален", false, true)
        chats[id] = chat
    }

    fun deleteMessage(id: Int, number: Int) {
        val chat = chats[id] ?: throw ChatNotFoundException(alert)
        chat.messages.filter { !it.delete }[number - 1].delete = true
        chats[id] = chat
    }

    fun getLastMessages(): List<String> {
        return chats.values.map { chat -> chat.messages.lastOrNull { !it.delete }?.text ?: alert }
    }

    fun getLastChatsNotRead() =
        chats.values.map { chat: Chat -> chat.messages.lastOrNull { !it.read }?.text ?: "нет непрочитанных сообщений" }

    fun getMessages(id: Int, count: Int): List<Message> {
        val chat = chats[id] ?: throw ChatNotFoundException(alert)
        return chat.messages.filter { !it.delete }.takeLast(count).onEach { it.read = true }
    }


}

class ChatNotFoundException(message: String) : RuntimeException(message)

data class Chat(

    var messages: MutableList<Message> = mutableListOf()
)

data class Message(

    val text: String = "text",
    var read: Boolean = false,
    var delete: Boolean = false
)

fun main() {

    val message = Message(text = "первое")
    val message1 = Message(text = "второе")
    val message2 = Message(text = "третье")
    val service = ChatService()
    service.addMessage(1, message)
    service.addMessage(2, message1)
    service.addMessage(2, message2)
    service.addMessage(3, message2)
    service.deleteMessage(1, 1)
    println(service.getLastMessages())
}


import org.junit.Test

import org.junit.Assert.*

class ChatServiceTest {

    private val service = ChatService()

    @Test
    fun addMessage() {

        val result = service.addMessage(1, Message())
        assertNotNull(result)
    }

    @Test
    fun printChats() {
        service.addMessage(1, Message())
        service.addMessage(1, Message())
        val result = service.printChats()
        assertEquals(service.chats, result)
    }

    @Test
    fun notPrintChats() {
        val result = service.printChats()
        assertEquals("Пока пусто", result)
    }

    @Test
    fun deleteChat() {
        service.addMessage(1, Message())
        service.addMessage(2, Message())
        service.deleteChat(2)
        val result = service.chats.getValue(2).messages[0].delete
        assertTrue(result)
    }

    @Test(expected = ChatNotFoundException::class)
    fun notDeleteChat() {
        val result = service.deleteChat(1)
    }

    @Test
    fun deleteMessage() {
        service.addMessage(1, Message())
        service.addMessage(1, Message())
        service.deleteMessage(1, 1)
        val result = service.chats.getValue(1).messages[0].delete
        assertTrue(result)
    }


    @Test(expected = ChatNotFoundException::class)
    fun notDeleteMessage() {
        val result = service.deleteMessage(1, 1)
    }

    @Test
    fun getLastMessages() {
        service.addMessage(1, Message())
        val result = service.getLastMessages().size
        assertEquals(1, result)
    }
    @Test
    fun getLastChatsNotRead() {
         service.addMessage(1, Message(read = true))
        val result = service.getLastChatsNotRead().joinToString ()
        assertEquals("нет непрочитанных сообщений", result)
    }
    @Test(expected = ChatNotFoundException::class)
    fun getMessagesException() {
        service.addMessage(1, Message())
        val result = service.getMessages(4, 1)
    }
    @Test
    fun getMessage(){
        service.addMessage(1, Message())
        service.addMessage(2, Message())
       val result=service.getMessages(1,1).size
        assertEquals(1, result)
    }
}


