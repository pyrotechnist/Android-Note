
## RecyclerView inside NestedScrollView

* Case 1 :  Few items on the RecyclerView      

1) You need to use support library 23.2.0 (or) above

2) and recyclerView height will be wrap_content.

3) recyclerView.setNestedScrollingEnabled(false)

But by doing this the recycler pattern don't work. (i.e all the views will be loaded at once because wrap_content needs the height of complete recyclerView so it will draw all recycler views at once. No view will be recycled). Try not to use this pattern util unless it is really required. Try to use viewType and add all other views that needs to scroll to recyclerView

(https://stackoverflow.com/questions/31000081/how-to-use-recyclerview-inside-nestedscrollview)

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
