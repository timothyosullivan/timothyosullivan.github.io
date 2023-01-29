---
categories: technical
date: "2022-01-29 16:12:00 +0100"
layout: post
title: Deploying Web Server Locally
---

In this entry, we will kick things off with getting a web server up and running locally. I am running on MacOS for reference. I am going to use Java as the runtime language for web server development and will be using Visual Studio Code as my IDE.

### 1. Install JDK

Install the Java Development Kit (JDK) on your local machine to support compiling and running your java code. For reference, I am running JDK v13.0.2 on my machine. You can check whether you already have the JDK installed by running the following command:

    javac -version

which will show content similar to below:

<figure>
<img
src="../media/rugby-blog-series-1.png"
alt="500x50" />
<figcaption aria-hidden="true">500x50</figcaption>
</figure>

This [guide](https://mkyong.com/java/how-to-install-java-on-mac-osx/) is a good reference for using brew to install JDK on the your Mac.
