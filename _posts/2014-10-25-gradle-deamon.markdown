---
layout: post
title:  "Speed up Gradle"
date:   2014-10-25 23:17:00
categories: gradle deamon
comments: yes
description: "Greadle deamon to speed things up when running Gradle tasks"
---


One thing you notice when you start using Gradle is the time it can take to run very simple tasks. Even something like listing available tasks using **gradle tasks** can take over 10 seconds. When you are developing you want everything as fast as possible, time is money. One of the best way to speed up Gradle is to use the Gradle deamon. What happens each time you run Gradle you are starting up a new JVM and that can take some time. Using the Gradle deamon we have a running JVM that we submit our jobs to.

To use the Gradle deamon add the following line to your **$HOME/.gradle/gradle.properties** (you may have to create it). 

{% highlight groovy %}
org.gradle.daemon=true
{% endhighlight %}

This makes all gradle projects use a deamon. You can also add this to project root gradle.properties file to only use the deamon for the selected project.

Few things to note. The daemon process automatically expires after 3 hours of idle time. So the next time you run Gradle it starts a new deamon. There are other times that a new Greadle deamon is forked, if a deamon is busy, if projects use different versions of gradle and more.

For a detailed information about the Gradle deamon see <a href="http://www.gradle.org/docs/current/userguide/gradle_daemon.html" target="_blank">http://www.gradle.org/docs/current/userguide/gradle_daemon.html</a>
