<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp"
    tools:context=".MainActivity">


    <ImageView
        android:id="@+id/avatar"
        android:layout_width="48dp"
        android:layout_height="48dp"

        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        tools:srcCompat="@sample/post_avatars" />

    <TextView
        android:id="@+id/textView"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginStart="16dp"
        android:ellipsize="end"
        android:maxLines="1"
        app:layout_constraintBottom_toTopOf="@+id/textView2"
        app:layout_constraintEnd_toStartOf="@+id/menu"
        app:layout_constraintStart_toEndOf="@+id/avatar"
        app:layout_constraintTop_toTopOf="@+id/avatar"
        tools:text="@sample/posts.json/data/author" />

    <TextView
        android:id="@+id/textView2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="16dp"
        app:layout_constraintBottom_toBottomOf="@+id/avatar"
        app:layout_constraintStart_toEndOf="@+id/avatar"
        app:layout_constraintTop_toBottomOf="@+id/textView"
        tools:text="@sample/posts.json/data/published" />

    <androidx.constraintlayout.widget.Barrier
        android:id="@+id/barrier"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:barrierDirection="bottom"
        app:barrierMargin="16dp"
        app:constraint_referenced_ids="avatar,textView2" />

    <TextView
        android:id="@+id/text"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="16dp"
        android:autoLink="web|all"
        android:lineSpacingExtra="8dp"
        app:layout_constraintTop_toBottomOf="@id/barrier"
        tools:text="@string/text" />

    <ImageButton
        android:id="@+id/menu"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:background="@android:color/transparent"
        android:importantForAccessibility="no"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="@+id/avatar"
        app:srcCompat="@drawable/menu" />

    <androidx.constraintlayout.widget.Barrier
        android:id="@+id/barrier2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:barrierDirection="bottom"
        app:barrierMargin="16dp"
        app:constraint_referenced_ids="text" />
</androidx.constraintlayout.widget.ConstraintLayout>



<resources>    
    <string name="app_name">MyHomeWork</string>
    <string name="text">Привет,это новая Нетология!Когда-то Нетология начиналась с интенсивов по онлайн-маркетингу.
        Затем появились курсы по дизайну,разработке,аналитике и управлению.Мы растём сами и помогаем расти студентам: от новичков до уверенных профессионалов.
    Но самое важное остаётся с нами: мы верим,что в каждом уже есть сила,которая заставляет хотеть больше,целиться выше,бежать быстрее.
    Наша миссия-помочь встать на путь роста и начать цепочку перемен- http//netolo.gy/fyb</string>
</resources>
