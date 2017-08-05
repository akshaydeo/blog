---
title: Basecamp vs JIRA
author: Deo Akshay
layout: post
permalink: /basecamp-vs-jira/
dsq_thread_id:
  - 1872879232
categories:
  - Tools
---
Being part of a start-up, I continuously look out for different products that are available to set up an end-to-end workflow in the company. Issue management and content management are one of the core parts of this workflow. If you google about &#8220;issue tracking&#8221;, &#8220;project management&#8221;, &#8220;content management&#8221; or similar search terms, you will be presented with a bunch of solutions. I am going to put my views on two of those solutions that we have used in [RainingClouds][1], [Basecamp][2] and [JIRA][3], and why I feel JIRA is much better than Basecamp, specifically for the software industry.

We started with Basecamp for [AppSurfer][4]. Initially it felt good. Minimal design, minimal clicks for surfing and easy to understand product. Single view for looking at all the issues/tasks of a project. We were part of private beta of new Basecamp. They made it more intuitive with good quality icons and visuals. But it still remains a generic task management system (which is their aim as far as I know). So it is not bad in that perspective. But as a software company Basecamp fails in some of the aspects :

  1. We used to report a lot of tasks and issues. Task list feature is fine but its like hell lot of information on single page
  2. Every task goes through stages like : Created | In progress | In Testing | Resolved | Build broken. No such facility is available in Basecamp
  3. Once an issue is closed it becomes part of archive which is difficult to go through.
  4. Source code changes associated with each task is really important concern. I used a third party tool [Zapier][5], but its too inconvenient and inconsistent, As there is no single point where I could manage all these things.
  5. Linking tasks is not possible as far as I understood Basecamp. Most of the times we faced a requirement to attach two issues / link two issues with each other.
  6. Documentation part is also not that good. Very little formatting tools, no feature of supporting code snippets in discussion and no facility to attach a document page (created within Basecamp) with existing tasks. (Thought it could be achieved by copy paste thing but its not expected)

After facing these issues, I started searching for a better solution and I came across Atlassian products. I was really impressed by two of their products [JIRA][3] and [Confluence][6]. So I started with their trial account, and bang !!. I am so impressed with them is that, we are almost sure about we are going to continue with them. There are a lot of features which are yet to be discovered but a few most obvious and appealing points are :

  1. They know the target audience hence provide all required features for task management like task workflow, task types, task linking etc.
  2. Customization of issue workflow is amazing. We need In Testing stage of an issue so as to show fix is ready for the issue and is under testing, that we achieved by customizing the workflow.
  3. We have our source code repositories on GitHub. JIRA provides a way to connect to my repositories on GitHub ( and also to BitBucket) and connect each of my code commits with issues. That gives me an issue with all the code changes associated with that issue on a single JIRA issue page. That is awesome for us !!
  4. Confluence, an amazing wiki replacement for us. We used wikimedia for some time but were not too happy with it. Confluence looks amazing and seamlessly integrates with JIRA. So a task has its wiki page associated with it. I love it !!
  5. Sharing of Spaces (that is what they call) is also a nice feature to have. All of the information can not be disclosed to all people. So this provides a nice way of privacy.
  6. They also provide nice analysis of issues, deadlines and combination of information which not much exciting but yes its useful.

I am still exploring these two products. But for sure if you are a software firm and confused which tool to use, I would recommend you to go for Atlassian products. At least try their trial period. And 99% of you wont regret it. 

**PS: This post contains my personal opinions and regarding current status of products at the time I was writing it.**

 [1]: http://rainingclouds.com
 [2]: http://basecamp.com
 [3]: http://www.atlassian.com/software/jira/overview
 [4]: http://apsurfer.com
 [5]: http://zapier.com
 [6]: http://www.atlassian.com/software/confluence/overview