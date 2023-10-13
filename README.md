# Задача №1. Только что

fun main() {

    val second = 120
    
    val min = second / 60
    
    val hour = min / 60

    println(agoToText(second, min, hour))

}

fun agoToText(second: Int, min: Int, hour: Int): String {

    var ago = when (second) {
    
        in 0..60 -> "был(а) только что"
        
        in 61..3599 -> "был(а) в сети $min ${calcMinute(min)} назад"
        
        in 3600..86399 -> "был(а) в сети $hour ${calcHour(hour)} назад"
        
        in 86400..172799 -> "был(а) в сети вчера"
        
        in 172800..259201 -> "был(а) в сети позавчера"
        
        else -> "был(а) в сети давно"
    }
    return ago
}

fun calcMinute(min: Int): String {

    var calcMin = when (min) {
    
        1, 21, 31, 41, 51 -> "минуту"
        
        in 2..4 -> "минуты"
        
        in 22..24 -> "минуты"
        
        in 32..34 -> "минуты"
        
        in 42..44 -> "минуты"
        
        in 52..54 -> "минуты"
        
        else -> "минут"
    }
    return calcMin
}

fun calcHour(hour: Int): String {

    var calcHours = when (hour) {
    
        2, 3, 4, 22, 23 -> "часа"
        
        in 5..20 -> "часов"
        else -> "час"
    }
    return calcHours
}

# Задача №2. Разная комиссия
fun main() {

    val prevTransition = 500_000.0
    val currentTransition = 80_000.0
    val cardType = "Maestro"

    val rez = countCommision(cardType, prevTransition, currentTransition)
    if (rez == -1.0) {
        println("Превышен лимит перевода по карте $cardType")
    } else
        println("Комиссия с суммы $currentTransition по карте $cardType $rez руб.")

}

fun countCommision(cardType: String, prevTransition: Double, currentTransition: Double): Double {

    val limitMessage = -1.0
    val minCoast = 35.0
    val onelimit = 150_000.0
    val oneLimitVk = 15_000.0
    val monthlyLimit = 600_000.0
    val monthLyLimitVk = 40_000.0
    var result = when (cardType) {
        "MasterCard", "Maestro" -> if (currentTransition < 75000.0) 0.0 else (currentTransition / 100 * 0.6) + 20.0
        "Visa", "Мир" -> if ((currentTransition / 100 * 0.75) < minCoast) minCoast else (currentTransition / 100 * 0.75)
        "Vk_pay" -> if (currentTransition > oneLimitVk || prevTransition > monthLyLimitVk) limitMessage else 0.0
        else -> 0.0
    }
    if (currentTransition > onelimit || prevTransition > monthlyLimit) result = limitMessage

    return result
}

}
