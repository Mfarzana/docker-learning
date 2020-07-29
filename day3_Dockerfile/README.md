## Goals
- Create Dockerfile 
- Build a Docker image from Dockerfile
- Run image
-  Push Java Web App In Docker Hub
- Pushing and Pulling to and from Docker Hub

## Push docker image (Spring demo project) to Docker Registry
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
 Image  uploaded 
 <img src="https://github.com/Mfarzana/docker-learning/blob/master/images/"></img>
 <img  
src=“images/demo-spring-dockerhub.jpg”  
raw=true  

/>
<img src="[/FahadAminShovon/kubeTest/raw/master/images/output.png](https://github.com/FahadAminShovon/kubeTest/raw/master/images/output.png)"
  


## Dockerfile
Docker builds images automatically by reading the instructions from a Dockerfile


## References
- https://docs.docker.com/develop/develop-images/dockerfile_best-practices/

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTE1NDQ0NjcyNywtNzcxNzA0Mzg4LC0yMD
k2MzIyODM2LDEzNzMxMDA2NTYsLTIxMTQxNDc3MDIsODEyNjg3
Mzk2LDc1Njc1NjE5NywtMjA3MzgwMjMxNiwxMjQ4NDA0OTgzLD
YyMzA0MDYzMyw4MTQwOTU5OTYsMTIzODU0Njc2LC0xMzA1NDAx
NzgzLC0zNTY0NDIwMzgsNDIyNTUwMjldfQ==
-->