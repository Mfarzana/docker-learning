## Goals
- Create Dockerfile 
- Build a Docker image from Dockerfile
- Run image
- Pushing and Pulling to and from Docker Hub

## Dockerfile
Dockerfile has two parts instruction and arguments. Docker builds images automatically by reading the instructions from a Dockerfile. 
- **FROM java:8-jdk-alpine**—  our image will be based on another image that is available on public repository (Docker Hub) that contains all **necessary dependencies** that we would need to run any **Java application**.


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

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExNjI0NTA2MDgsLTIxMjc0NjAzNjAsMT
cxOTM2MzU4NCwxNDMxOTY3ODIsOTA0MzgyMDc1LC01ODI5MTYy
ODYsMTM3NzIzMjM4MCwxNzAwODU5NzkzLC0xNjIwMDEyNDQ0LD
YyMDcyOTkwNiwxMzUxMTYyNzg5LDEwMzIxMTI3NTMsLTExMDMw
NzQ2NzcsLTc3MTcwNDM4OCwtMjA5NjMyMjgzNiwxMzczMTAwNj
U2LC0yMTE0MTQ3NzAyLDgxMjY4NzM5Niw3NTY3NTYxOTcsLTIw
NzM4MDIzMTZdfQ==
-->