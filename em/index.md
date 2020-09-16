---
title: "🧑‍💻 Engineering Management"
layout: default
group: em
---

<h1>Engineering Management Blog</h1>
<hr
        style="
            background-image: linear-gradient(to right, #eeeeee, white);
            margin-top: 15px;
            margin-bottom: 15px;
            border: 0;
            height: 1px;
        "
    />

[Engineering Management](https://en.wikipedia.org/wiki/Engineering_management#:~:text=Engineering%20management%20is%20a%20career,of%20complex%20engineering%20driven%20enterprises.) is a tough job. More so because, most of the EMs are not trained. They get trained on the field,
where there are bullets coming at em from all the sides. In my first stint, which was my own startup, I had no formal
training of Engineering Management (not even People management). And I sucked big time. I was a bad manager, bad team
member and bad CTO. 

This remained the same until I moved back to IC and realized that
**It's not just you, but your team, product and your customers suffers because your mistakes.** There are some great engineering managers like [Will Larson](https://lethain.com/), [Michael Lopp](https://randsinrepose.com/) who are quite vocal about the common mistakes and takeaways for improving your management styles. But one thing was missing from all the material I have read till now (and I have read very little 🙈) : **Implementation**!

[An Elegant Puzzle ]() is one of the best books I have read on this topic.

<div class="container">
    <div class="row">
        <div class="posts">
            {% for post in site.em_posts %}
            <div class="post">
                <h1 class="post-title">
                    <a href="{{ post.url }}"> {{ post.title }} </a>
                </h1>
                <div class="row" style="margin-top: 0rem">
                    <div class="col-12" style="margin-top: 1rem">
                        <span class="post-date" style="font-size: 15px"> {{ post.date | date_to_string }} </span>
                    </div>
                </div>
                <div class="row">
                    <!-- <div class="d-flex col-md-3 col-sm-12 align-items-center" style="padding-bottom:10px;">
                      <img class="rounded align-self-center" style="margin:auto;" src="{{ post.image }}" />
                    </div> -->
                    <div class="col-sm-12">{{ post.excerpt }}</div>
                </div>                
            </div>
            {% endfor %}
        </div>
    </div>
</div>
