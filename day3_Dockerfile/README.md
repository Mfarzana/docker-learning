## Goals
- Create Dockerfile 
- Build a Docker image from Dockerfile
- Run image
- Pushing and Pulling to and from Docker Hub

## Dockerfile
Dockerfile has two parts instruction and arguments. Docker builds images automatically by reading the instructions from a Dockerfile. 
- **FROM java:8-jdk-alpine**— Every Dockerfile typically starts with a FROM line. This FROM command receives as argument a basic existent docker image that we will use to build our layers on top of and it contains all **necessary dependencies** that we would need to run any **Java application**.


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
eyJoaXN0b3J5IjpbMTIyNDcyOTI3MiwtMTE2MjQ1MDYwOCwtMj
EyNzQ2MDM2MCwxNzE5MzYzNTg0LDE0MzE5Njc4Miw5MDQzODIw
NzUsLTU4MjkxNjI4NiwxMzc3MjMyMzgwLDE3MDA4NTk3OTMsLT
E2MjAwMTI0NDQsNjIwNzI5OTA2LDEzNTExNjI3ODksMTAzMjEx
Mjc1MywtMTEwMzA3NDY3NywtNzcxNzA0Mzg4LC0yMDk2MzIyOD
M2LDEzNzMxMDA2NTYsLTIxMTQxNDc3MDIsODEyNjg3Mzk2LDc1
Njc1NjE5N119
-->