# card_post.xml
     <com.google.android.material.button.MaterialButton
        android:id="@+id/menu"
        style="@style/Widget.AppTheme.MenuButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:importantForAccessibility="no"
        app:icon="@drawable/condition_of_menu"
        app:layout_constraintBottom_toBottomOf="@+id/avatar"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

         <com.google.android.material.button.MaterialButton
        android:id="@+id/likes"
        style="@style/Widget.AppTheme.LikeButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checkable="true"
        app:icon="@drawable/condition_of_like"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/barrier2" />

        <com.google.android.material.button.MaterialButton
        android:id="@+id/repost"
        style="@style/Widget.AppTheme.ShareButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checkable="true"
        app:icon="@drawable/condition_of_share"
        app:layout_constraintBottom_toBottomOf="@+id/likes"
        app:layout_constraintStart_toEndOf="@+id/likes"
        app:layout_constraintTop_toTopOf="@+id/likes" />

# drawable 
    condition_of_menu.xml
    
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/menu" />
    </selector>

    condition_of_share.xml

    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">  
    <item android:drawable="@drawable/baseline_forward_to_inbox_24" />
    </selector>

    condition_of_like.xml

    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/baseline_favorite_24" android:state_checked="true" />
    <item android:drawable="@drawable/baseline_favorite_border_24" />
    </selector>

# color
    like_tint.xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:color="#ff0000" android:state_checked="true" />
    <item android:color="?attr/colorControlNormal" />
    </selector>

    menu_tint.xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:color="?attr/colorControlNormal" />
    </selector>

    share_tint.xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:color="?attr/colorControlNormal" />
    </selector>
# themes.xml
    <resources xmlns:tools="http://schemas.android.com/tools">
    <!-- Base application theme. -->
    <style name="Base.Theme.MyHomeWork" parent="Theme.Material3.DayNight.NoActionBar">
      <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
    </style>

    <style name="Widget.AppTheme.LikeButton" parent="Widget.Material3.Button.TextButton.Icon">
        <item name="iconTint">@color/like_tint</item>
        <item name="backgroundTint">@android:color/transparent</item>
        <item name="iconSize">@dimen/size24dp</item>
    </style>

    <style name="Widget.AppTheme.ShareButton" parent="Widget.Material3.Button.TextButton.Icon">
        <item name="iconTint">@color/share_tint</item>
        <item name="backgroundTint">@android:color/transparent</item>
        <item name="iconSize">@dimen/size24dp</item>
    </style>

    <style name="Widget.AppTheme.MenuButton" parent="Widget.Material3.Button.IconButton">
        <item name="iconTint">@color/menu_tint</item>
        <item name="backgroundTint">@android:color/transparent</item>
        <item name="iconSize">@dimen/size24dp</item>
    </style>
    </resources>

    
  

