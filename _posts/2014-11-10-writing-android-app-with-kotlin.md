---
title: Writing Android app with Kotlin
date: 2014-11-10 00:00:00 Z
permalink: "/writing-android-app-with-kotlin/"
tags:
- Android
- Kotlin
tags:
- Android
- Kotlin
author: Deo Akshay
layout: post
post_to_facebook_timeline:
- 1
comments: true
dsq_thread_id:
- 3208572681
image: https://symbols.getvecta.com/stencil_86/44_kotlin-icon.444573255c.svg
---

When I read about Kotlin, I thought of it as a functional port of Java which is (<span style="text-decoration: underline;"><em>100% interoperable with Java</em></span>). I have been playing with Scala for a bit. The biggest trouble I had with Scala is wide range of semantic alternatives. But it seems like, Kotlin has a decisive approach towards making Java more functional and more compact for developers.

<span>Kotlin does support Androi and they do have a very verbose documentation about Android support</span>, So I went ahead and tried one simple list app with an adapter and a fragment, just to simulate a simple part of general Android application.

**Setup**

- Install Kotlin plugin available in Preferences for Android Studio.
- Add following classpath to the gradle script
  {% highlight groovy %}classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:0.9.206"{% endhighlight %}

- And add the dependency in the application gradle script
  {% highlight groovy %}compile "org.jetbrains.kotlin:kotlin-stdlib:$kotlin_version"
  {% endhighlight %}

**The Application**

I started with writing a HackerNews listing app with simulated API calls (had to do it quickly).

**Code**

Let's dive into some code.

**Home**

It's the main activity (launcher) of the application.
{% highlight java %}
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
{% endhighlight %}

**Things I loved**

- Kotlin has val, var keywords to separate out the final and variables.
- Nullable is always appended with a &#8216;?&#8217;. That hints the compulsions around null checks and method calls. Helps to avoid null pointers :-)

**PostListFragment**

This is the fragment that contains list of posts.

{% highlight java %}
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
{% endhighlight %}

**Things I loved**

- <span style="text-decoration: underline;">Functions are first class citizens</span>.<span style="text-decoration: underline;"> And this helps us to remove all those interfaces we end up creating to circumvent this problem in Java.</span>
- In Kotlin, you have to write all the static methods separately inside an object. Most of the times the static methods are factory methods, and this helped me to make the code more readable. So I call each of the object inside the class as Maker, and write different factory method inside it. This is completely possible with Java by writing inner classes, but this being the semantics of the language, forces us to write the factory methods like this.

**Post**  
This is the model class containing info related with each post.

{% highlight java %}
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
{% endhighlight %}

**Things I loved**

- Data classes have amazing inherent toString support. Great for logging
- Compact, more readable declarations for model classes.

**PostListAdapter**

This is the list adapter for post list view.

{% highlight java %}
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
{% endhighlight %}

This all works well. The source code is available at <https://github.com/akshaydeo/kotlin_android>.

**Some analysis of APK**

- The very important thing to look at is, <span style="text-decoration: underline;">the number of classes inside your APK</span>. Compared to Java based app, its gonna be huge. <span style="text-decoration: underline;">For example the application that I have created contains <strong>3561</strong> classes</span>.<span style="text-decoration: underline;">Running proguard to strip out the unused classes is a must before creting a release build</span>.
- Huge memory requirements during the builds. So you will have to change the VM preferences before using Kotlin as your Android development laguage. Mine were: {% highlight groovy %}-XX:MaxPermSize=1024m -XX:MaxHeapSize=256m -Xmx256m
  {% endhighlight %}

**Important Links for Kotlin**

- [Website][1]
- [Language References][2]

I will be updating this post as I progress in analysing the side effects of using Kotlin for Android development.

[1]: http://kotlinlang.org/
[2]: http://kotlinlang.org/docs/reference/
