## Goals
- Pushing and Pulling to and from Docker Hub

- **Spring Boot Demo Project Structure:**
- ![enter image description here](https://github.com/Mfarzana/docker-learning/blob/master/images/demo-project-structure.jpg)
## Push docker image to Docker Hub Registry

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
  
**Step 1: Install Docker**
```
ubuntu@ip-172-31-4-174:~$ sudo apt update
ubuntu@ip-172-31-4-174:~$ sudo apt install docker.io


```
## Dockerfile
Dockerfile has two parts instruction and arguments. Docker builds images automatically by reading the instructions from a Dockerfile. 
- **FROM java:8-jdk-alpine** Our image will be based on another image that is available on public repository (Docker Hub) and **this image  contains all necessary dependencies** that we would need to run any **Java application**.

- **COPY ./target/demo.war /usr/app/**: First argument after COPY (/target/demo.war) is a **path of an application** that we want to put into container. The second parameter, /usr/app/ , is a **directory in a container** where we put the app.

- **WORKDIR /usr/app**: Use **/usr/app** folder as a **root**.
- **EXPOSE 8080** — Container will **listen** to **specific port**

- **ENTRYPOINT ["java", "-jar", "demo.war"]** —  Run the application, where first value is a command and the last two are parameters.




## References
- https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
- https://medium.com/@wkrzywiec/how-to-put-your-java-application-into-docker-container-5e0a02acdd6b
- https://hub.docker.com/_/openjdk
- https://medium.com/@migueldoctor/how-to-create-a-custom-docker-image-with-jdk8-maven-and-gradle-ddc90f41cee4

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTExODE0MTYxNCw5OTc1Mzg4MTIsLTE5OD
I5MzE5NDMsMTY3MDM3MTU3MSwxMTMxODIwNDcwLC0xNzQyNzA3
NTA5LDEyMjQ3MjkyNzIsLTExNjI0NTA2MDgsLTIxMjc0NjAzNj
AsMTcxOTM2MzU4NCwxNDMxOTY3ODIsOTA0MzgyMDc1LC01ODI5
MTYyODYsMTM3NzIzMjM4MCwxNzAwODU5NzkzLC0xNjIwMDEyND
Q0LDYyMDcyOTkwNiwxMzUxMTYyNzg5LDEwMzIxMTI3NTMsLTEx
MDMwNzQ2NzddfQ==
-->