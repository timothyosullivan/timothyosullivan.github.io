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
<img src="2023-01-29-deploying-web-server-locally-media/c680a8a7bc19e14a3a71489aa88b95d84f6bc253.png" style="width:250px;height:50px;" />
</figure>

This <a href="https://mkyong.com/java/how-to-install-java-on-mac-osx/">guide</a> is a good reference for using brew to install JDK on the your Mac.
