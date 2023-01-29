This blog series will document the experiences of building a rugby focused website that enables users to:

	- browse historical rugby results
	- search for statistics related to a specific game 
	- visualize and analyse team performance
	- sign up to receive result notifications for their favourite team
	- Predict team performance in an upcoming match based on historical results

The reason for choosing rugby as an area of focus is simply because I love watching rugby so it makes it a natural product candidate for me!

Accompanying the build out of product features will be a easy to follow step-by-step guide on 

So, let's get going then. Click Rugby Series 1 - Deploying a web server locally to move to the first stage of getting our rugby site up and running.


- Installed JDK and Maven
- Used Spring Boot to create Java web application
	- Maven command to build application
		- mvn clean install 
	- Maven command to initiate web server locally running the application
		- ./mvnw spring-boot:run 
	- View running application
		- Navigate to browser and enter URL: http://localhost:8080/
	-  Build a deployable JAR file
		-  mvn clean install
- Specified infrastructure using Cloud Formation using WebServer.yaml
	- Ref. https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/working-with-templates-cfn-designer-walkthrough-createbasicwebserver.html
- Deployed on AWS manually
- Use Docker to build abd deploy containerized spring boot application
	- https://aws.plainenglish.io/run-containerized-app-on-aws-ec2-using-cloudformation-f067182952c6
- 
