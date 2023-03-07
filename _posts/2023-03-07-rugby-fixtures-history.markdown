---
categories: technical
date: "2023-03-07 16:12:00 +0100"
layout: post
title: Rugby Fixtures History
---

<span style="border-radius: 10px; background: WhiteSmoke; padding: 10px; text: black">
    <span style="color: DarkSlateGray">
        <b>
            &#9432; previous entry in the blog series can be found <a href="https://timosullivan.org/first-cloud-deployment/">here</a>
        </b>
    </span>
</span>

In this entry, the target is to display a page that will show all historical international rugby results. To get this up and going, we need to do the following:

1. Update our IaC script to incorporate an RDS PostgreSQL instance
2. On the default path, populate a 'match' results table in PostgreSQL DB by referencing a csv file dump of match records
3. Use <a href="https://www.thymeleaf.org/">Thymeleaf</a> as a server-side Java template engine to present a html page listing the rugby match results

### 1. Add RDS PostgreSQL to CloudFormation Script

The first task is introduce parameters to capture user preferences for the name of the DB instance, the master user name, and the master user password. This is depicted in the screenshot below:

<figure>
    <img src="../media/rugby-blog-series-6.png" width="auto" height="200px"/>
</figure>

Secondly, in the 'Resources' section of the YAML file, we need to indicate that an RDS DB instance needs to be created. The specification of the DB instance references the user inputed parameters in terms of DB instance name, master user name, and master user password. Additionally, we specify the DB engine (Postgres) and server instance (db.t3.micro) to use. 

<figure>
    <img src="../media/rugby-blog-series-7.png" alt="200x30" />
</figure>

In the above screenshot, a VPC security group is also attached to the DB instance. This is the third important aspect to specify in the IaC script for establishing access to the DB instance. As per the below screenshot, the RDS instance will accept inbound Postgres TCP / IP connections over port 5432 from the security group associated with our EC2 web server instance.

<figure>
    <img src="../media/rugby-blog-series-8.png" alt="200x30" />
</figure>

Finally, we update our 'user data' ec2 instance bootstrap script to include commands to export the RDS DB connection details to associated environment variables on instance startup. This is important as these details are needed when establishing the connection to the DB instance from the running web server java code.

<figure>
    <img src="../media/rugby-blog-series-9.png" alt="200x30" />
</figure>


### 2. Populate Match Results Table

Now that we have the DB instance created, the next step is to get historical rugby match results captured in a table in the running database. This could be done as part of a standalone data migration script. As this exercise is primarily for learning purposes and the DB instance won't be running continuously, we're instead going to opt to have table creation done once the web server instance default path is first hit by a user. This obviously means that we need to first check whether table already exists, so that we do not try to create again.

To coordinate the load of historical match results, we introduce a new service class called 'HistoricalResultsService' which will be responsible for establishing a connection to the RDS DD datasource and then:

    1. Checking the tables listing in the DB Information Schema to determine 
       if a 'match' table already exists
    2. If the table does not exist, then the service will upload the contents 
       of a csv file dump to a newly created 'match' table 

The below screenshot shows the logic for populating the 'match' table:

<figure>
    <img src="../media/rugby-blog-series-10.png" alt="200x60" />
</figure>

### 3. Display Match Results on UI

We are going to use <a href="https://en.wikipedia.org/wiki/Jakarta_Persistence">JPA</a> to manage the persistence of relational data for the rugby application. Firstly, we need to create a 'match' entity to correspond to the 'match' table in the database. Instances of this entity will correspond to individual rows in the table. 

The @Entity annotation declares that the class represents an entity. @Id declares the attribute which acts as the primary key of the entity.

<figure>
    <img src="../media/rugby-blog-series-11.png" alt="200x20" />
</figure>

We are using <a href="https://spring.io/projects/spring-data-jpa">Spring Data JPA</a> offered by the Spring Java Application Framework for the JPA repository implementation. This supports all necessary CRUD operations.

Next up, we create a 'MatchRepository' interface which extends the Spring JPA 'CrudRepository'. Additionally, we create a 'MatchService' with a single method (for now) that returns all instances of the type 'Match' from the respository i.e. this will retrieve all match records from the database table.

Now we are on the home stretch as the last task is to simply present these records in a table on a HTML page. 

We use <a href="https://www.thymeleaf.org/">Thymeleaf</a> as a server-side Java template engine to present a html page listing the rugby match results. This is quite straight forward and we simply reference the relevant 'Match' entity attributes in the HTML table definition as per the screenshot below:

<figure>
    <img src="../media/rugby-blog-series-12.png" alt="200x50" />
</figure>

Finally, we make sure everything is working as expected by navigating to the relevant path on our browser and checking the table is shown as expected:

<figure>
    <img src="../media/rugby-blog-series-13.png" alt="200x120" />
</figure>

All looking good but it would be nice if we could improve the user experience of finding the match results that most interests them e.g. perhaps they have a particular interest in a specific team in a year e.g. How did Ireland perform in 1995? So that's our next task, let's get a search engine capability deployed. To see how this is done, please jump to the next blog entry!

<span style="border-radius: 10px; background: WhiteSmoke; padding: 10px; text: black">
    <span style="color: DarkSlateGray">
        <b>
            &#9432; next entry in the blog series can be found <a href="https://timosullivan.org/introducing-search/">here</a>
        </b>
    </span>
</span>