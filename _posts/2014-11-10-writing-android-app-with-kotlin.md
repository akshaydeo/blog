---
title: Writing Android app with Kotlin
author: Deo Akshay
layout: post
permalink: /writing-android-app-with-kotlin/
post_to_facebook_timeline:
  - 1
comments: true
dsq_thread_id:
  - 3208572681
categories:
  - Android
  - Kotlin
tags:
  - Android
  - Kotlin
---
When I read the description about kotlin, I thought of it as a very beautiful port of Java (<span style="text-decoration: underline;"><em>as its a JVM based language and 100% interoperable with Java</em></span>). I have been trying to like Scala for a lot time now, but hell lot of options for everything, complex semantics makes the code unreadable and confusing (<span style="text-decoration: underline;"><em>for me</em></span>). But kotlin, according to me is a decisive approach towards making Java more functional and more compact for developers.

The first thing I searched when I read about Kotlin was, about Android support. <a title="Scaloid" href="https://github.com/pocorall/scaloid/" target="_blank">Scaloid</a> being more and more popular and I being completely not in love with Scala, this was going to be an interesting thing for me. <span style="text-decoration: underline;">Kotlin does have support for Android and they do have amazing documentation about both language as well as Android support</span>, So I went ahead and tried one simple list app with an adapter and a fragment, just to simulate a simple part of general Android application.

**Setup**

  * Install Kotlin plugin available in Preferences for Android Studio.
  * Add following classpath to the gradle script 
    <pre>classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:0.9.206"
</pre>

  * And add the dependency in the application gradle script 
    <pre>compile "org.jetbrains.kotlin:kotlin-stdlib:$kotlin_version"
</pre>

**The Application**

I started with writing a HackerNews listing app. But then restricted it to simulating the API call with a stub so as to focus onto the Kotlin more than the actual functionality of the application.

**Code**

Let&#8217;s dive into some code.

**Home**

It&#8217;s the main activity (launcher) of the application.
```java Home Activity
open class Home(): ActionBarActivity(){

    val logTag = "###Home###"

    override fun onCreate(savedInstanceState: android.os.Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_home)
        val transaction = getSupportFragmentManager().beginTransaction()
        transaction.replace(R.id.posts_container, PostListFragment.Maker.Create())
        transaction.commit()
    }
}
```

**Things I loved**

  * Kotlin has val, var keywords to separate out the final and variables (if we compare it with Java). I personally feel that val and var are the best ways to indicate that if a member is final or not.
  * Nullable is always appended with a &#8216;?&#8217;. So this is again a very informative way of indicating the possible exceptions that can happen when you are working with a particular member or parameter.

**PostListFragment**

This is the fragment that contains list of posts. It&#8217;s a DialogFragment, so as to make it available to be used as a dialog as well.

``` java Post List Fragment
open class PostListFragment() : DialogFragment() {
    val logTag = "###PostListFragment###"
    var postList: ListView? = null
    var baseView: View? = null
    var postsAdapter: PostListAdapter? = null
    var handler: Handler? = null

    object Maker {
        fun Create(): PostListFragment {
            return PostListFragment()
        }
    }

    override fun onCreateView(inflater: LayoutInflater?, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        baseView = inflater?.inflate(R.layout.fragment_post_list, container, false)
        postList = baseView?.findViewById(R.id.post_list) as ListView
        postsAdapter = PostListAdapter(getActivity(), postList as AbsListView)
        postList.setAdapter(postsAdapter)
        handler = Handler()
        return baseView
    }

    override fun onResume() {
        super.onResume()
        Post.Maker.GetList {
            Log.d(logTag,"Testing get list =&gt;" + it.get(0).post)
            postsAdapter?.postList?.addAll(it)
            handler?.post {
                this.postsAdapter?.notifyDataSetChanged()
            }
        }
    }
}
```

**Things I loved**

  * <span style="text-decoration: underline;">The biggest advantage Kotlin gives you is passing functions as parameter (Functions are first class citizens)</span>.<span style="text-decoration: underline;"> And this helps us to remove all those interfaces we end up creating to circumvent this problem in Java. (Look at the Post.Maker.GetList)</span>
  * In Kotlin, you have to write all the static methods separately inside an object. Most of the times the static methods are factory methods, and this helped me to make the code more readable. So I call each of the object inside the class as Maker, and write different factory method inside it. This is completely possible with Java by writing inner classes, but this being the semantics of the language, forces us to write the factory methods like this.

**Things I hated**

  * <span style="text-decoration: underline;">&#8216;?&#8217; comes really handy to avoid null pointer exceptions. But I feel that also is a problematic.</span> In most of the cases during development, null pointer exceptions help us to fix issues quickly. Now as this was a small skit, I never faced any null pointer exception but while writing some complex code this could make a few things really tricky.

**Post**  
This is the model class containing info related with each post.

``` java Post Model
data class Post(val id: Long, val post: String, val by: String, val postedAt: Date) {
    object Maker {
        fun GetList(onSuccess: (posts: List) -&gt; Unit) {
            val posts = ArrayList()
            for (a in 1..10) {
                posts.add(Post(a.toLong(), "Test", "Akshay", Date()))
            }
            onSuccess(posts)
        }
    }
}
```

**Things I loved**

  * Making class as a data class makes it easy to debug. Using instance of this class in logs prints the values of each of the field of the class in readable format. I feel this is one of the best thing. This can be done in Java by overriding toString() method.
  * The way you define members of the class, is much much compact and better. Now my model classes will just have the Maker which will have the factory methods.
  * <span style="text-decoration: underline;"><strong>Functions are first class citizens!</strong></span> Look at the way GetList is coded. It accepts a function, that takes up List of Post as param and returns nothing. (Unit in Kotlin means nothing). So forget all the interfaces we used to write for circumventing the problem of passing functions as arguments.

**PostListAdapter**

This is the list adapter for post list view.

```java List Adapter
open class PostListAdapter(val context:Context,val listView: AbsListView): BaseAdapter(){

    private val logTag: String = "###PostListAdapter###"
    var postList = ArrayList()

    override fun getCount(): Int {
        return postList.size
    }

    override fun getItem(position: Int): Any? {
        if(position &gt;= postList.size)
            return null
        return postList.get(position)
    }

    override fun getItemId(p0: Int): Long {
        if(p0 &gt;= postList.size)
            return -1
        return postList.get(p0).id
    }

    override fun getView(position: Int, view: View?, parent: ViewGroup): View? {
        var cachedItem: ItemCache?
        var contentView: View ? = view
        when (contentView) {
            null -&gt; {
                contentView = LayoutInflater.from(context).inflate(R.layout.layout_post_item, parent, false)
                cachedItem = ItemCache(textView = contentView?.findViewById(R.id.post) as TextView)
                contentView?.setTag(cachedItem)
            }
            else -&gt; {
                cachedItem = contentView?.getTag() as ItemCache
            }
        }
        val currentPost = getItem(position) as Post
        cachedItem?.textView?.setText(currentPost.post)
        return contentView
    }

    class ItemCache(val textView: TextView?)
}
```

This all works well. The source code is available at <https://github.com/akshaydeo/kotlin_android>.

**Some analysis of APK**

  * The very very important thing to look at is, <span style="text-decoration: underline;">the number of classes inside your APK</span>. Don&#8217;t worry, its gonna be huge that the APK you will create with same functionality using JAVA. <span style="text-decoration: underline;">For example the application that I have created contains <strong>3561</strong> classes</span>. (I am planning to write the same app using Java and compare the results with Kotlin app). <span style="text-decoration: underline;">So running proguard during the debug builds is very essential in this case</span>.
  * Also I faced gradle crashing problem because of the default VM params. So you will have to change them using preferences. I changed them to <pre class="">-XX:MaxPermSize=1024m -XX:MaxHeapSize=256m -Xmx256m
</pre>

**Important Links for Kotlin**

  * [Website][1]
  * [Language References][2]

I will be updating this post as I progress in analysing the side effects of using Kotlin for Android development.

 [1]: http://kotlinlang.org/
 [2]: http://kotlinlang.org/docs/reference/
