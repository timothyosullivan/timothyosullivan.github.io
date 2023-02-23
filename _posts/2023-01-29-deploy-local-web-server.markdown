---
categories: technical
date: "2023-01-29 16:12:00 +0100"
layout: post
title: Deploy Local Web Server
---

<span style="border-radius: 10px; background: WhiteSmoke; padding: 10px; text: black">
    <span style="color: DarkSlateGray">
        <b>
            &#9432; previous entry in the blog series can be found <a href="https://timosullivan.org/rugby-series/">here</a>
        </b>
    </span>
</span>

In this entry, we will kick things off with getting a web server up and running locally. I am running on MacOS for reference. I am going to use Java as the runtime language for web server development and will be using Visual Studio Code as my IDE.

### 1. Install JDK

Install the Java Development Kit (JDK) on your local machine to support compiling and running your java code. For reference, I am running JDK v13.0.2 on my machine. You can check whether you already have the JDK installed by running the following command:

    javac -version

which will show content similar to below:

<figure>
<img
123
alt="400x30" />
<figcaption aria-hidden="true">
400x30
</figcaption>
</figure>

This <a href="https://mkyong.com/java/how-to-install-java-on-mac-osx/">guide</a> is a good reference for using brew to install JDK on a Mac OS.

### 2. Install Maven

<a href="https://maven.apache.org/">Maven</a> is a build automation tool for Java projects so the next step is to download and install Maven on your development machine. This <a href="https://mkyong.com/maven/install-maven-on-mac-osx/#homebrew-install-maven-on-macos">guide</a> is a good reference for using brew to install Maven on a Mac OS.

You can confirm Maven is correctly installed and you have environment variable path configured by running the following command:

    mvn -version

This should return a response like the below:

<figure>
<img
123
alt="650x70" />
<figcaption aria-hidden="true">
650x70
</figcaption>
</figure>

### 3. Install Spring Boot

<a href="https://spring.io/">Java Spring Framework</a> is an open source, enterprise-level framework for creating standalone, production-grade applications that run on the Java Virtual Machine (JVM).

<a href="https://spring.io/projects/spring-boot">Spring Boot</a> is a tool that makes developing web application and microservices with Spring Framework faster and easier through three core capabilities:

1.  Autoconfiguration
    - Autoconfiguration means that applications are initialized with pre-set dependencies that you don't have to configure manually.
2.  An opinionated approach to configuration
    - Spring Boot uses an opinionated approach to adding and configuring starter dependencies, based on the needs of your project.
3.  The ability to create standalone applications
    - Sprint Boot enables engineers to develop standalone applications that run on their own, without relying on an external web server, by embedding a web server such as Tomcat or Netty into your app during the initialization process.

These features work together to provide you with a tool that allows you to set up a Spring-based application with minimal configuration and setup \[ref. <a href="https://www.ibm.com/topics/java-spring-boot#:~:text=Spring%20Boot%20helps%20developers%20create,app%20during%20the%20initialization%20process.">IBM</a>\].

### 4. Build Sample Application

Referencing this <a href="https://spring.io/guides/gs/spring-boot/">Sprint Boot quick start guide</a>, clone this <a href="https://github.com/spring-guides/gs-spring-boot.git">git repository</a>, navigate to the 'complete' folder within the repository and perform a build of the codebase by executing the following command:

    mvn -clean install

This command will compile the sources, execute any applicable JUnit tests and package the compiled files into a JAR file in the 'target' folder.

The console output should show:
<p>
    <span style="color: MediumSeaGreen"><b>Build Success</b></span>
</p>

### 5. Run Sample Application

To run the sample application, execute the following command:

    ./mvnw spring-boot:run

This instantiates a web server that is listening on port 8080 which can be hit by running the following curl command from another terminal window:

<figure>
<img
123
alt="400x30" />
<figcaption aria-hidden="true">
400x30
</figcaption>
</figure>

The response from the @RestController class will provide the initial base for building out the rugby statistics website but first, we want to get the server set up and running in the cloud. To see how this is done, let's keep things moving and jump to the next blog entry!

<span style="border-radius: 10px; background: WhiteSmoke; padding: 10px; text: black">
    <span style="color: DarkSlateGray">
        <b>
            &#9432; next entry in the blog series can be found <a href="https://timosullivan.org/first-cloud-deployment/">here</a>
        </b>
    </span>
</span>
