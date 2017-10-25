
## RecyclerView inside NestedScrollView

* Case 1 :  Few items on the RecyclerView, set height of RecyclerView as wrap_content, and setNestedScrollingEnabled(false)
            Don't use this if there are many items, it will draw all recycler views at once. No view will be recycled

Layout :

```
<android.support.v4.widget.NestedScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fillViewport="true"
    tools:context="com.longyuan.zhihuretrofitvolley.MainActivity">

    <LinearLayout
        android:background="@android:color/holo_blue_bright"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <android.support.v4.view.ViewPager
            android:id="@+id/viewPage_topStories"
            android:layout_width="match_parent"
            android:layout_height="500px" />

            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:text="Latest news" />

            <android.support.v7.widget.RecyclerView
                android:id="@+id/news_list"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:background="@color/colorPrimaryGray">

            </android.support.v7.widget.RecyclerView>

    </LinearLayout>

</android.support.v4.widget.NestedScrollView>

```

Activity :

```
        mStoryList.setNestedScrollingEnabled(false);
```
