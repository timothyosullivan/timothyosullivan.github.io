---
categories: technical
date: "2023-02-22 16:12:00 +0100"
layout: post
title: First Cloud Deployment
---

<span style="border-radius: 10px; background: WhiteSmoke; padding: 10px; text: black">
    <span style="color: DarkSlateGray">
        <b>
            &#9432; previous entry in the blog series can be found <a href="https://timosullivan.org/deploy-local-web-server/">here</a>
        </b>
    </span>
</span>

In this entry, we will are going to get the local web server that we got running previously, to be hosted on the cloud. The cloud provider that we will use for this blog series will be AWS.

### 1. Infrastructure as Code

We are going to use Infrastructure as Code (IaC) to specify our cloud infrastructure requirements which will give our software product these benefits:

1.  Reproducibility and Consistency
    - By using code, we ensure the infrastructure is consistently configured and deployed the same way every time. This helps eliminate inconsistencies and reduce the risk of configuration errors.
2.  Version Control
    - IAC leverages version control systems such as Git, making it easier to track changes to our infrastructure over time.
3.  Automation
    - IAC makes it possible to automate infrastructure provisioning and configuration, reducing the need for manual tasks and reducing potential for errors.
4.  Improved Disaster Recovery
    - IAC improves disaster recovery management by enabling infrastructure recreation in the event of failure.
5.  Cost Savings
    - IAC leads to cost savings, as it reduces the time and effort required to provision and manage infrastructure, and can help eliminate the costs associated with manual errors and inconsistencies.

### 2. Create CloudFormation Script

<a href="https://aws.amazon.com/cloudformation/">CloudFormation</a> (CF) is an AWS service that enabling modeling, provisioning, and managing AWS and third-party resources by treating infrastructure as code.

Let's create the CF template that will specify the server instance to host our web server. The CF service will use this <a href="https://github.com/timothyosullivan/rugby/blob/master/complete/IaC/infrastructure.yaml">template</a> to instantiate the infrastructure stack with the resources specified.

Our initial CF script will look to configure the following resources:

- EC2 instance (t2.micro) running latest Amazon Linux 2 OS
  - Deployed within subnet in default VPC
  - Only accepting HTTP and SSH connections
- Spring Boot web application server running on the EC2 instance

#### 2.1 Applicaton Bootstrap Script

The application bootstrap script will execute the rugby web application once the server is up and running.

Firstly, the jar file executable needs to be uploaded to an S3 bucket to enable the EC2 instance to retrieve the file when it is ready to deploy the rugby web app.

    aws s3 cp target/rugby-app-1.0-SNAPSHOT.jar s3://{your bucket name}/rugby-app-1.0-SNAPSHOT.jar

The shell script commands for the deployed EC2 server instance need to be encoded as base64 text as per the code snippt below.

    UserData:
        Fn::Base64:
            !Sub |
                #!/bin/bash
                aws s3 cp s3://${S3BucketName}/rugby-app-1.0-SNAPSHOT.jar /home/ec2-user/rugby-app-deployment-1.0-SNAPSHOT.jar
                sudo yum -y install java-17
                cd /home/ec2-user/
                sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-ports 8080
                java -jar rugby-app-deployment-1.0-SNAPSHOT.jar > rugby-app-deployment.log

The above is pretty self-explanatory but essentially the script will copy the executable Jar for the specified S3 bucket to the EC2 instance. It will then install the specified Java version and proceed to launch the executable Spring Boot rugby web app.

### 3. Validate Deployment

The web app deployment can be easily tested by either navigating to the public ip of the EC2 instance as per the screenshot below:

<figure>
    <img src="../media/rugby-blog-series-4.png" alt="200x30" />
</figure>

or by simply executing a curl command on the terminal window:

<figure>
    <img src="../media/rugby-blog-series-5.png" alt="200x30" />
</figure>

Now that we have the web server up and running in the cloud, let's make the content served more meaningful for rugby fans by displaying a history of all international rugby matches. To see how this is done, let's keep things moving and jump to the next blog entry!

<span style="border-radius: 10px; background: WhiteSmoke; padding: 10px; text: black">
    <span style="color: DarkSlateGray">
        <b>
            &#9432; next entry in the blog series can be found <a href="https://timosullivan.org/rugby-fixtures-history/">here</a>
        </b>
    </span>
</span>
