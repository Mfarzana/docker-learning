## Goals
- Create Dockerfile 
- Build a Docker image from Dockerfile
- Run image
- Pushing and Pulling to and from Docker Hub

## Dockerfile
Dockerfile has two parts instruction and arguments. Docker builds images automatically by reading the instructions from a Dockerfile. 
- **FROM java:8-jdk-alpine**— Every Dockerfile typically starts with a FROM line. Here we tell Docker that our image will be based on another image that is available on public repository (Docker Hub) and **this image  contains all necessary dependencies** that we would need to run any **Java application**.

- **COPY ./target/demo.war /usr/app/** — First argument after COPY (/target/demo.war ) is a path of an application that we want to put into container. I’m using .war file because my project is a web application, but you if yours is a standard .jar app go with that. The second parameter, /usr/app/ , is a directory in a container where we put the app.


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
eyJoaXN0b3J5IjpbMTE4NzcwMzg2NiwxMjI0NzI5MjcyLC0xMT
YyNDUwNjA4LC0yMTI3NDYwMzYwLDE3MTkzNjM1ODQsMTQzMTk2
NzgyLDkwNDM4MjA3NSwtNTgyOTE2Mjg2LDEzNzcyMzIzODAsMT
cwMDg1OTc5MywtMTYyMDAxMjQ0NCw2MjA3Mjk5MDYsMTM1MTE2
Mjc4OSwxMDMyMTEyNzUzLC0xMTAzMDc0Njc3LC03NzE3MDQzOD
gsLTIwOTYzMjI4MzYsMTM3MzEwMDY1NiwtMjExNDE0NzcwMiw4
MTI2ODczOTZdfQ==
-->