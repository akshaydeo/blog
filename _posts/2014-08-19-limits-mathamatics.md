---
title: Limits (Mathamatics)
author: Deo Akshay
layout: post
comments: true
permalink: /limits-mathamatics/
post_to_facebook_timeline:
  - 0
dsq_thread_id:
  - 2941087828
categories:
  - Mathamatics
---
*[Warning: These are more like my notes which I make public so that if at all there are some stupid people like me, these would come handy]*

**Mathematical definition:**  
A limit is the value that a function or sequence &#8220;approaches&#8221; as the input or index approaches some value.[1]

Limit of function

Mathematical Representation:

{% highlight math %}
lim  ƒ(x) = L
x->c
{% endhighlight %}

This says that L is the value of function ƒ(x) when x reaches to value c.

There is one more definition which is kind of logical evidence to this is L is the limit of function ƒ(x) with ε as a small positive real number then value of ƒ(x) lies in (L-ε,L+ε) or |ƒ(x) &#8211; L| < ε

Reallife examples:  
I found this amazing <a href="http://math.stackexchange.com/questions/365770/what-is-a-simple-example-of-a-limit-in-the-real-world about real-life examples of limit." title="question" target="_blank">question</a> 

**Adding the most upvoted example here**

The reading of your speedometer (e.g., 85 km/h) is a limit in the real world. Maybe you think speed is speed, why not 85 km/h. But in fact your speed is changing continuously during time, and the only &#8220;solid&#8221;, i.e., &#8220;limitless&#8221; data you have is that it took you exactly 2 hours to drive the 150 km from A to B. The figure your speedometer gives you is at each instant t0 of your travel the limit

{% highlight math %}
     v(t0):= lim    (t0)−s(t0−Δt)/Δt
            Δt→0s
{% endhighlight %}

where s(t) denotes the distance travelled up to time t.

**And the most simple to understand (for me)**

If I keep tossing a coin as long as it takes, how likely am I to never toss a head?

Possibility of getting a head is (1/2) if I toss the coin once.

Now if I toss N times P(N) = (1/2)^N  
Now this probability would be zero if N is ∞.

Mathematical representation of the question is 

P(N) = (1/2)^N find N for which P(N) = 0

and Answer to the given question is  
{% highlight math %}
lim  P(N)
N->∞
{% endhighlight %}
