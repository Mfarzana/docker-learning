## Goals
- Create Dockerfile 
- Build a Docker image from Dockerfile
- Run image
- Pushing and Pulling to and from Docker Hub

## Dockerfile
Dockerfile has two parts instruction and arguments. Docker builds images automatically by reading the instructions from a Dockerfile. 
- **FROM java:8-jdk-alpine**— with this line we tell Docker that our image will be based on another image that is available on public repository (Docker Hub). This image was prepared by someone else and contains all necessary dependencies that we would need to run any Java application.

Every Dockerfile typically starts with a FROM line. This FROM command receives as argument a basic existent docker image that we will use to build our layers on top of. This image  contains all **necessary dependencies** that we would need to run any **Java application**.


This is the starting point for your Dockerfile. E  The base image passed as argument is openjdk:8-jdk-alpine. This image contains a jdk version 8 already installed. The alpine version means that the image makes use of the alpine distribution, which is significantly smaller than any other Linux distribution.

## Push docker image to Docker Hub Registry
- **Spring Boot Demo Project Structure:**
- ![enter image description here](https://github.com/Mfarzana/docker-learning/blob/master/images/demo-project-structure.jpg)
#### Step 1: Go to project directory and create a docker file 
 For example, my project was in desktop demo folder
  ```
  C:\Users\Farzana>cd Desktop\demo
  ```
  **My  Dockerfile** 
	
	FROM java:8-jdk-alpine
	COPY ./target/demo.war /usr/app/
	WORKDIR /usr/app
	EXPOSE 8080
	ENTRYPOINT ["java", "-jar", "demo.war"]
	
 **NB:  . (dot) mean current directory** 
####  Step 2: Create docker image from Dockerfile
 ```
 C:\Users\Farzana\Desktop\demo>docker build -t mfarzana/demo-spring:0.0.1 . 
 ```

#### Step 3: Push image to Docker Hub
```
C:\Users\Farzana\Desktop\demo>docker login 
#Put username and password and use this command
C:\Users\Farzana\Desktop\demo>docker push mfarzana/demo-spring:0.0.1
 ```
 Image  uploaded: ![](https://github.com/Mfarzana/docker-learning/blob/master/images/demo-spring-dockerhub.jpg)
  




## References
- https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
- https://medium.com/@wkrzywiec/how-to-put-your-java-application-into-docker-container-5e0a02acdd6b
- https://hub.docker.com/_/openjdk
- https://medium.com/@migueldoctor/how-to-create-a-custom-docker-image-with-jdk8-maven-and-gradle-ddc90f41cee4

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTkyMjc4MTQ0OSwxMjI0NzI5MjcyLC0xMT
YyNDUwNjA4LC0yMTI3NDYwMzYwLDE3MTkzNjM1ODQsMTQzMTk2
NzgyLDkwNDM4MjA3NSwtNTgyOTE2Mjg2LDEzNzcyMzIzODAsMT
cwMDg1OTc5MywtMTYyMDAxMjQ0NCw2MjA3Mjk5MDYsMTM1MTE2
Mjc4OSwxMDMyMTEyNzUzLC0xMTAzMDc0Njc3LC03NzE3MDQzOD
gsLTIwOTYzMjI4MzYsMTM3MzEwMDY1NiwtMjExNDE0NzcwMiw4
MTI2ODczOTZdfQ==
-->