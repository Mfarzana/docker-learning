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
#Put username and password and use Command
C:\Users\Farzana\Desktop\demo>docker push mfarzana/demo-spring:0.0.1
 ```
 Image 
 
  


## Dockerfile
Docker builds images automatically by reading the instructions from a Dockerfile


## References
- https://docs.docker.com/develop/develop-images/dockerfile_best-practices/

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg2OTIxNDk3MiwtMjA5NjMyMjgzNiwxMz
czMTAwNjU2LC0yMTE0MTQ3NzAyLDgxMjY4NzM5Niw3NTY3NTYx
OTcsLTIwNzM4MDIzMTYsMTI0ODQwNDk4Myw2MjMwNDA2MzMsOD
E0MDk1OTk2LDEyMzg1NDY3NiwtMTMwNTQwMTc4MywtMzU2NDQy
MDM4LDQyMjU1MDI5XX0=
-->