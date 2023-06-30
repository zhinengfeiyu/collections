CoordinatorLayout相关

配合AppBarLayout的常规写法：
<androidx.coordinatorlayout.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <com.google.android.material.appbar.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@color/transparent"
        app:elevation="0dp">

        <View
            android:id="@+id/v_scroll_range"
            android:layout_width="match_parent"
            android:layout_height="150dp"
            android:background="@color/red_ff4444"
            app:layout_scrollFlags="scroll" />

        <View
            android:id="@+id/v_solid_range"
            android:layout_width="match_parent"
            android:layout_height="100dp"
            android:background="@color/green" />

    </com.google.android.material.appbar.AppBarLayout>

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/main_recycler_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior"/>

</androidx.coordinatorlayout.widget.CoordinatorLayout>


AppBarLayout相关：

AppBarLayout是一个orientation为vertical的LinearLayout，使用时不用再设置orientation

getTotalScrollRange()获得当前可滑动最大距离，频繁调用性能很高，因为内部会缓存上次计算得到的数值。在页面初始化时该方法返回0，但是在offsetChanged回调里该方法一定会返回正确的数值

无法直接获得当前已滑动的距离，必须通过下面的滑动监听，记录verticalOffset，该记录一定是准确的，因为AppBarLayout内部也是这样记录的
mAppBarLayout.addOnOffsetChangedListener { appBarLayout, verticalOffset ->
    …
}
滑动前初始化时就会触发该回调，测试下来会回调两次为0的verticalOffset
verticalOffset的含义：如果当前可滑动区域完全展示，即初始状态，verticalOffset为0；上滑后verticalOffset变为负值，一直递减到-getTotalScrollRange()


AppBarLayout一般需要设置:
android:background="@color/transparent"
app:elevation="0dp"
因为默认自带白色背景和阴影

在上面的例子中，设置了appbar_scrolling_view_behavior的高度为match_parent的RecyclerView，其实际高度为父布局高度减去v_solid_range的高度，并没有减去v_scroll_range的高度，所以初始状态RecyclerView底部有一部分是在屏幕外的，直到可滑动部分完全滑出才会展示全
