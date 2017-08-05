---
title: Consumer Producer implementation with golang
author: Deo Akshay
layout: post
comments: true
permalink: /consumer-producer-implementation-with-golang/
post_to_facebook_timeline:
  - 1
dsq_thread_id:
  - 1840577645
categories:
  - go
---
Today I invested my whole day in evaluating GoLang for one of our component implementations. Since the first document, I got impressed by it'&#8217;'s construct. Till I reached the ultimate problem solution for our case, I fell in love with this new language. It gives you the power of OS level constructs keeping a lot of overheads hidden from you. I was playing with a lot of simple problems while reading the documentation and here I am sharing one of those implementation and will discuss how simple it is to implement pretty complex problems with [GoLang][1].

**Classic Consumer-Producer problem**  
![](//4.bp.blogspot.com/-9UpZKfJbugY/UFYe4MpX7II/AAAAAAAAAIw/57jLbqLQn6c/s1600/producer_consumer.png)

You can find some classic implemetations [here][2].

GoLang implementation:

{% highlight go %}
package main
import (
	"fmt"
	"strconv" // added for giving proper name to job
	"time"    // added for delaying consecutive inserts into job queue
)
func main() {
	jobQueue := make(chan string, 200)
	done := make(chan bool,1)
	// consumer
	go func() {
		for {
			job := <-jobQueue
			fmt.Println("Got job:", job)
		}
		done <- true
	}()
	// producer
	go func() {
		i := int64(0)
		for {
			jobQueue &lt;- strconv.FormatInt(i, 10)
			time.Sleep(time.Second * 5) // to wait for 5 seconds before adding one more job
			i++
		}
		done <- true
	}()
	<-done
}
{% endhighlight %}

**Components**

**Channels**  
Channels are the basic inter go-routine communication model. If you are a Java developer you can consider it as array blocking queue, which is mostly used for inter-thread communication. 

**Go routine**  
Go routines are the lightweight Thread implementation for Go.

You can learn Go with some amazing examples [here][3]. Initial impressions of Go are pretty good. Will share more info as we discover it during our implementations.

 [1]: http://golang.org
 [2]: http://en.wikipedia.org/wiki/Producer%E2%80%93consumer_problem#Examples
 [3]: https://gobyexample.com/
