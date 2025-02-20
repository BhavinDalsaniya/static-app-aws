# static-app-aws

# Steps to Set Up and Deploy the Spring Boot Application

## 1. Create a VPC and Subnet
- First, create a VPC in any one region with the CIDR block `10.0.0.0/16`.
- After that, create a subnet within the VPC with the CIDR block `10.0.1.0/24`.

## 2. Create and Attach an Internet Gateway
- Create an Internet Gateway (IGW) for accessing the internet.
- Attach the IGW to the created VPC.

## 3. Update the Route Table
- Add the Internet Gateway to the route table associated with the VPC.
- Ensure the route table has the following entries:
  - `10.0.0.0/16` for local traffic.
  - `0.0.0.0/0` with the Internet Gateway for external traffic.

## 4. Connect to Your EC2 Instance
- Use the following command to connect to your EC2 instance via SSH:

1.  cd Downloads
2.  ssh -i first-ec2.pem ubuntu@54.88.58.168

## 5. Clone the repository:
3.mkdir repos
4.cd repos/
5.git clone https://github.com/rafsan-jany/spring-boot-rest-api-h2-database.git
6.cd spring-boot-rest-api-h2-database


## 6. Install Required Software
Update the package list and install Java 17 and Maven:

sudo apt update
sudo apt install openjdk-17-jdk -y
sudo apt install maven -y

## 7. Verify the installations:
java --version
mvn --version

## 8. Build the Application
Build the application using Maven:

mvn clean install
This will generate a .jar file inside the target folder.

## 9. Run the Application
Execute the JAR file:
java -jar target/spring-embedded-h2-db-0.0.1-SNAPSHOT.jar

By default, Spring Boot runs on port 8080. To check if the application is running:
lsof -i :8080

## 10. Configure Security Group
Ensure that port 8080 is allowed in the inbound rules of your EC2 instance's security group.

## 11. Access the Application
Open your browser and navigate to:

http://54.88.58.168:8080/student
Initially, you will see a blank page because no data has been added yet.

## 12. Enable Remote Access to H2 Console
Stop the running application using Ctrl+C.

Edit the application.properties file and add the following line:
spring.h2.console.settings.web-allow-others=true

Save the file and rebuild the application:
mvn clean install
java -jar target/spring-embedded-h2-db-0.0.1-SNAPSHOT.jar

Access the H2 console at:
http://54.88.58.168:8080/h2-console/

Use the following details to connect:
URL: jdbc:h2:mem:rjanytest
Username: sa
Password: 123

## 13. Add Data to the Application

Use a tool like ReqBin to send a POST request to:
http://54.88.58.168:8080/student

Use the following JSON body:
{
  "id": 1,
  "name": "Bhavin",
  "age": 27,
  "email": "demo@gmail.com"
}

Verify the data by visiting:
http://54.88.58.168:8080/student
To fetch a specific student by ID, use:

http://54.88.58.168:8080/student/1
