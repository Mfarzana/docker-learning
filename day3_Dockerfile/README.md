## Goals
- Create Dockerfile 
- Build a Docker image from Dockerfile
- Run image
-  Push Java Web App In Docker Hub
- Pushing and Pulling to and from Docker Hub

## Publish docker image (Spring demo project) to Docker Registry
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
#Put username and password and use Command
C:\Users\Farzana\Desktop\demo>docker push mfarzana/demo-spring:0.0.1
 ```
 
  


## Dockerfile
Docker builds images automatically by reading the instructions from a Dockerfile


## References
- https://docs.docker.com/develop/develop-images/dockerfile_best-practices/

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTM3MzEwMDY1NiwtMjExNDE0NzcwMiw4MT
I2ODczOTYsNzU2NzU2MTk3LC0yMDczODAyMzE2LDEyNDg0MDQ5
ODMsNjIzMDQwNjMzLDgxNDA5NTk5NiwxMjM4NTQ2NzYsLTEzMD
U0MDE3ODMsLTM1NjQ0MjAzOCw0MjI1NTAyOV19
-->