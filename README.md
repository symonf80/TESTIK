<?xml version="1.0" encoding="utf-8"?>

    <androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:padding="@dimen/size16dp">

    <ImageView
        android:id="@+id/avatar"
        android:layout_width="@dimen/size48dp"
        android:layout_height="@dimen/size48dp"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:srcCompat="@drawable/ic_launcher_foreground" />

    <TextView
        android:id="@+id/author"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginStart="@dimen/size16dp"
        android:ellipsize="end"
        android:maxLines="1"
        app:layout_constraintBottom_toTopOf="@+id/published"
        app:layout_constraintEnd_toStartOf="@+id/menu"
        app:layout_constraintStart_toEndOf="@+id/avatar"
        app:layout_constraintTop_toTopOf="@+id/avatar"
        tools:text="@sample/posts.json/data/author" />

    <TextView
        android:id="@+id/published"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="@dimen/size16dp"
        app:layout_constraintBottom_toBottomOf="@+id/avatar"
        app:layout_constraintStart_toEndOf="@+id/avatar"
        app:layout_constraintTop_toBottomOf="@+id/author"
        tools:text="@sample/posts.json/data/published" />

    <androidx.constraintlayout.widget.Barrier
        android:id="@+id/barrier"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:barrierDirection="bottom"
        app:barrierMargin="@dimen/size16dp"
        app:constraint_referenced_ids="avatar,published" />


    <TextView
        android:id="@+id/content"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="@dimen/size16dp"
        android:autoLink="web|all"
        android:lineSpacingExtra="8dp"
        app:layout_constraintTop_toBottomOf="@id/barrier"
        tools:text="@sample/posts.json/data/content" />

    <com.google.android.material.button.MaterialButton
        android:id="@+id/menu"
        style="@style/Widget.AppTheme.MenuButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:importantForAccessibility="no"
        app:icon="@drawable/condition_of_menu"
        app:layout_constraintBottom_toBottomOf="@+id/avatar"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="@+id/avatar" />

    <androidx.constraintlayout.widget.Barrier
        android:id="@+id/barrier2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:barrierDirection="bottom"
        app:barrierMargin="@dimen/size16dp"
        app:constraint_referenced_ids="content" />

    <com.google.android.material.button.MaterialButton
        android:id="@+id/likes"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        style="@style/Widget.AppTheme.LikeButton"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/barrier2"
        android:checkable="true"
        app:icon="@drawable/condition_of_like" />


    <com.google.android.material.button.MaterialButton
        android:id="@+id/share"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="@dimen/size16dp"
        style="@style/Widget.AppTheme.ShareButton"
        android:checkable="true"
        app:layout_constraintStart_toEndOf="@+id/likes"
        app:layout_constraintTop_toBottomOf="@id/barrier2"
        app:icon="@drawable/condition_of_share" />

    <ImageView
        android:id="@+id/views"
        android:layout_width="32dp"
        android:layout_height="32dp"
        android:layout_marginEnd="8dp"
        app:layout_constraintBottom_toBottomOf="@+id/share"
        app:layout_constraintEnd_toStartOf="@+id/tvViews"
        app:layout_constraintTop_toTopOf="@+id/share"
        app:srcCompat="@drawable/views" />

    <TextView
        android:id="@+id/tvViews"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintBottom_toBottomOf="@+id/views"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="@+id/views"
        tools:text="555" />

     </androidx.constraintlayout.widget.ConstraintLayout>

     ----------------------------------------------------------------

     <?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"

    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".view.FeedFragment">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/list"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layoutManager="androidx.recyclerview.widget.LinearLayoutManager"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        tools:listitem="@layout/card_post" />


    <com.google.android.material.floatingactionbutton.FloatingActionButton
        android:id="@+id/add"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="16dp"
        android:importantForAccessibility="no"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:srcCompat="@drawable/baseline_add_24" />
     </androidx.constraintlayout.widget.ConstraintLayout>
