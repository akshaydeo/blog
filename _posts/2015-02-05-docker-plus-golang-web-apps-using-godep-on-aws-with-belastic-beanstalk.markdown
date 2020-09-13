---
title: Docker + Golang web apps using Godep on AWS with Elastic Beanstalk using CodeShip
date: 2015-02-05 11:59:29 Z
layout: post
comments: true
image: https://blog.golang.org/go-brand/Go-BB_cover.jpg
---

> This post is pretty old, and I no longer use any of the tech mentioned in this post and won't recommend anyone to use. (Except Go, which I still love the most, probably more)

A lot of things in one title right :D. We are coming up with a small tool for developers that is going to help them to distribute mobile application releases easily, and mainly during development phase. Initially we were using the free dyno provided by Heroku, as staging environment. But as the date of release is approaching, it was time to move onto more scalable (heroku is scalable but becomes a bit costly when you start using it for production purpose) and cheaper infrastructure.

## Golang web application structure

My web application is based on slightly customised version of [Gin](https://github.com/go-go/gin) + PostgreSQL at the backend, AngularJS + Bootstrap 3 + Custom CSS for front end. I have been using [Godep](https://github.com/tools/godep) for dependency management on Heroku. And I feel, this is one of the better ways than creating a bash script with all the go get.
**If any of the dependencies have introduced some changes that breaks the existing apps, then you are kind of fucked.** Godep basically works on the commit id of the code that you have checked out while development.

**Note 1:** Also a lot of people on the forums suggested me to go for precompiled binary to reduce the size of the docker images. I haven't given a try to it. You have to use a few ready made tools to create binary for your production environment.

**Note 2:** If you are not sure which ORM you should use,then give a try to [Gorm](https://github.com/jinzhu/gorm).

## First thing first, Install boot2docker
As you will first test everything locally, you will need [boot2docker](http://docs.docker.com/installation/). And just initialise it with following commands
{% highlight bash %}
boot2docker init
boot2docker start
{% endhighlight %}

## Creating a Dockerfile
{% highlight conf %}
# Using wheezy from the official golang docker repo
FROM golang:1.4.1-wheezy

# Setting up working directory
WORKDIR /go/src/<project>
Add . /go/src/<project>/

# Get godeps from main repo
RUN go get github.com/tools/godep

# Restore godep dependencies
RUN godep restore

# Install
RUN go install <project>

# Setting up environment variables
ENV ENV dev

# My web app is running on port 8080 so exposed that port for the world
EXPOSE 8080
EXPOSE
ENTRYPOINT ["/go/bin/<project>"]
{% endhighlight %}

Most of the things in this Dockerfile are self explanatory ([List of commands](https://docs.docker.com/reference/builder/)).

Golang team is kind enough to create the debian images with the latest Golang versions available. So the first line means that I intend to run this in a container with OS Debian(wheezy) with Golang 1.4.1.

## What about the DB connection ?
So in my DEV environment I had a postgres instance running locally. I struggled a bit for allowing my docker container to connect the database on the host but it was taking a lot of time. So for a quick fix, I created a new app on heroku, added a free postgres addon.

## Building the repository
So we have a Dockerfile, now the next step is to build the repository for the container and running it. Your working directory is the root of your project.
{% highlight bash %}
docker build -t <project_name> .
{% endhighlight %}
You will see all of the commands that we have added in the Docker file will start executing one by one. At the end if everything goes well you shall see `Successfully built <ID>`

If you see this message then we are all set to deploy project on the cloud. For testing you can publish the repository with following command and test it from local browser

{% highlight bash %}
docker run --publish 3000:8080 --name <tag-name> --rm <project-name>
{% endhighlight %}

Check your docker container ip address using following command
{% highlight bash %}
boot2docker ip
{% endhighlight %}
And now go to your browser and enter address as http://ip_address:3000 and everything should work.

## Setting up continous integration
I went through a couple of CI tools supporting Golang web apps and settled down onto <a href="http://codeship.com" target="_blank">Codeship</a>. (I am evaluating <a href="http://shppable.com" target="_blank">Shippable</a>).

For my Go projects, I don't keep up with the regular naming conventions i.e. github.com/rainingclouds/project_name. So I had to work a bit on the test scripts as Codeship assumes you are following the naming conventions. (The reason I don't follow it because, the project is not supposed to be reused as it is by anyone else :))
{% highlight bash %}
mv ../distribute ${HOME}/src/distribute
cd ${HOME}/src/distribute
go get github.com/tools/godep
godep restore
go get
{% endhighlight %}
And then you can put your testing commands. I am not using any framework for writing tests. Default testing package is more than enough for me at this stage.

{% highlight bash %}
go test <module_to_test> -v
{% endhighlight %}

## Setting up the deployment hook
Codeship comes up with Elastic beanstalk deployment hook. For that you have to configure a few things before you can use it.

### Configuring ElasticBeanstalk environment

#### Create IAM user for codeship
I keep on using different users for these different services, So that we can manage the access independently. I created a user with all access for Elastic Beanstalk. There is a ready template for this in AWS IAM panel.
#### Create a new application
Create a new application in elastic beanstalk and don't select a demo application option. You will see a Create Application button on right top of the panel. For this you will have to upload a zip of your source code manually.

**Note** : I am also using ELB in front of my EC2 instances. For https, I am doing the SSL termination at LB level, and then redirecting it to my Golang application.

#### Create a new environment
Create a new environment inside your new application.
### Configure codeship deployment hook
Once you have configured AWS, go to codeship and put all the information in AWS Beanstalk deployment settings page.

## Done
If all goes well then the entire pipeline is ready. For testing make a small change and push it to your configured branch. You will see that the new task in the codeship with the exact status of it :).
