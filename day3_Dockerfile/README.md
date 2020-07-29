## Goals
- Create Dockerfile 
- Build a Docker image from Dockerfile
- Run image
-  Push Java Web App In Docker Hub
- Pushing and Pulling to and from Docker Hub

## Push docker image to Docker Registry
- **Spring Boot Demo Project Structure** 
- 
![enter image description here](https://github.com/Mfarzana/docker-- learning/blob/master/images/demo-project-structure.jpg)

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
 C:\Users\Farzana\Desktop\demo>**docker build -t mfarzana/demo-spring:0.0.1 . 
 ```

#### Step 3: Push image to Docker Hub
```
C:\Users\Farzana\Desktop\demo>docker login 
#Put username and password and use this command
C:\Users\Farzana\Desktop\demo>docker push mfarzana/demo-spring:0.0.1
 ```
 Image  uploaded: ![](https://github.com/Mfarzana/docker-learning/blob/master/images/demo-spring-dockerhub.jpg)
  


## Dockerfile
Docker builds images automatically by reading the instructions from a Dockerfile


## References
- https://docs.docker.com/develop/develop-images/dockerfile_best-practices/

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2MjAwMTI0NDQsNjIwNzI5OTA2LDEzNT
ExNjI3ODksMTAzMjExMjc1MywtMTEwMzA3NDY3NywtNzcxNzA0
Mzg4LC0yMDk2MzIyODM2LDEzNzMxMDA2NTYsLTIxMTQxNDc3MD
IsODEyNjg3Mzk2LDc1Njc1NjE5NywtMjA3MzgwMjMxNiwxMjQ4
NDA0OTgzLDYyMzA0MDYzMyw4MTQwOTU5OTYsMTIzODU0Njc2LC
0xMzA1NDAxNzgzLC0zNTY0NDIwMzgsNDIyNTUwMjldfQ==
-->