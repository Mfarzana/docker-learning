## Goals
- Create Dockerfile 
- Build a Docker image from Dockerfile
- Run image
-  Push Java Web App In Docker Hub

## Push Java Web App In Docker Hub Repository 
#### Step 1: Go to project directory and create a docker file 
> For example my project was in desktop demo folder
  ```
  C:\Users\Farzana>cd Desktop\demo
  ```
  **My  Dockerfile** 
	``` 
	FROM java:8-jdk-alpine
	COPY ./target/demo.war /usr/app/
	WORKDIR /usr/app
	EXPOSE 8080
	ENTRYPOINT ["java", "-jar", "demo.war"]
	```
 **NB:  .(dot) mean current directory** 
####  Step 2: Use command 
> - C:\Users\Farzana\Desktop\demo>**docker build -t mfarzana/demo-spring:0.0.1 .**
> - This command create docker image from Dockerfile
#### Step 3: Push image to Docker Hub
> - C:\Users\Farzana\Desktop\demo>docker login
> - Put username and password
> - Use Command
>> - C:\Users\Farzana\Desktop\demo>**docker push mfarzana/demo-spring:0.0.1**
 
  


## Dockerfile
Docker builds images automatically by reading the instructions from a Dockerfile


## References
- https://docs.docker.com/develop/develop-images/dockerfile_best-practices/

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTU0Njc4OTkxMCw3NTY3NTYxOTcsLTIwNz
M4MDIzMTYsMTI0ODQwNDk4Myw2MjMwNDA2MzMsODE0MDk1OTk2
LDEyMzg1NDY3NiwtMTMwNTQwMTc4MywtMzU2NDQyMDM4LDQyMj
U1MDI5XX0=
-->