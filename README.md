plugins {

    id 'org.jetbrains.kotlin.jvm' version '1.8.10'
    id 'application'
    id 'jacoco'
}

group = 'org.example'
version = '1.0-SNAPSHOT'

repositories {

    mavenCentral()
}

dependencies {

    implementation 'org.jetbrains.kotlin:kotlin-stdlib'
    testImplementation'junit:junit:4.13.2'

}



kotlin {

    jvmToolchain(8)
}

application {

    mainClassName = 'MainKt'
}


package ru.netology

import org.junit.Assert.*
import org.junit.Test


class MainKtTest {

    @Test
    
    fun countCommisionForMaestro() {
        val prevTransition = 0.0
        val currentTransition = 70_000.0
        val cardType = "Maestro"

        val result = countCommision(cardType, prevTransition, currentTransition)
        assertEquals(0.0, result)
    }
    @Test
    
    fun countCommisionForMaestroLimit() {
        val prevTransition = 0.0
        val currentTransition = 80_000.0
        val cardType = "Maestro"

        val result = countCommision(cardType, prevTransition, currentTransition)
        assertEquals(500.0, result)
    }

    @Test
    
    fun countCommisionForVisa() {
        val prevTransition = 0.0
        val currentTransition = 80_000.0
        val cardType = "Visa"

        val result = countCommision(cardType, prevTransition, currentTransition)
        assertEquals(600.0, result)
    }
    @Test
    fun countCommisionForVisaLimit() {
        val prevTransition = 0.0
        val currentTransition = 30.0
        val cardType = "Visa"

        val result = countCommision(cardType, prevTransition, currentTransition)
        assertEquals(35.0, result)
    }

    @Test
    
    fun countCommisionForDefolt() {
        val prevTransition = 0.0
        val currentTransition = 5_000.0
        val cardType = "Vk_Pay"


        val result = countCommision(cardType, prevTransition, currentTransition)
        assertEquals(0.0, result)
    }
    @Test
    
    fun countCommisionForDefoltLimitDay() {
        val prevTransition = 0.0
        val currentTransition = 16_000.0
        val cardType = "Vk_Pay"


        val result = countCommision(cardType, prevTransition, currentTransition)
        assertEquals(-1.0, result)
    }
    @Test
    
    fun countCommisionForDefoltLimitMonth() {
        val prevTransition = 41_000.0
        val currentTransition = 5_000.0
        val cardType = "Vk_Pay"
        val result = countCommision(cardType, prevTransition, currentTransition)
        assertEquals(-1.0, result)
    }
    @Test
    
    fun countCommisionForOneLimit() {
        val prevTransition = 0.0
        val currentTransition = 170_000.0
        val cardType = "Maestro"

        val result = countCommision(cardType, prevTransition, currentTransition)
        assertEquals(-1.0, result)
    }
    @Test
    
    fun countCommisionForMonthlyLimit() {
        val prevTransition = 700_000.0
        val currentTransition = 50_000.0
        val cardType = "Maestro"

        val result = countCommision(cardType, prevTransition, currentTransition)
        assertEquals(-1.0, result)
    }
}

data class Post(

    val id: Int,
    val ownerId: Int,
    val fromId: Int,
    val date: Int,
    val text: String,
    val comments: Comments,
    val copyright: String,
    val likes: Likes,
    val canPin: Boolean,
    val isFavorite: Boolean
) {

    data class Comments(
        val count: Int,
        val canPost: Boolean,
        val groupsCanPost: Boolean,
        val canClose: Boolean,
        val canOpen: Boolean
    )

    data class Likes(
        val count: Int,
        val userLikes: Boolean,
        val canLike: Boolean,
        val canPublish: Boolean
    )
}

class WallService {

    private var idUniqu = 1;
    private var posts = emptyArray<Post>()
    fun add(post: Post): Post {
        posts += post
        return posts.last()
    }

    fun update(post: Post): Boolean {
        TODO()
    }

}
fun main() {

    val comments = Post.Comments(1, true, true, true, true)
    val likes = Post.Likes(0, true, true, false)
    val post = Post(2, 1, 1, 1, "text", comments, "copy", likes, true, true)
    val wall = WallService()
    println(wall.add(post))
}
