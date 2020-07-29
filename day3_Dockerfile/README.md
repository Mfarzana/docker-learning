## Goals
- Create Dockerfile 
- Build a Docker image from Dockerfile
- Run image
-  Push Java Web App In Docker Hub

## Push Java Web App In Docker Hub Repository 
#### Step 1: Go to project directory and create a docker file 
> For example my project was in desktop demo folder
  - **C:\Users\Farzana>cd Desktop\demo**
 >  **Dockerfile** 
	FROM java:8-jdk-alpine
	COPY ./target/demo.war /usr/app/
	WORKDIR /usr/app
	EXPOSE 8080
	ENTRYPOINT ["java", "-jar", "demo.war"]
 **NB:  .(dot) mean current directory** 
### Step 2: Use comman
> C:\Users\Farzana\Desktop\demo>**docker build -t mfarzana/demo-spring:0.0.1 .**
 
  


## Dockerfile
Docker builds images automatically by reading the instructions from a Dockerfile


## References
- https://docs.docker.com/develop/develop-images/dockerfile_best-practices/

<!--stackedit_data:
eyJoaXN0b3J5IjpbNjA1MTg5MDIzLC0yMDczODAyMzE2LDEyND
g0MDQ5ODMsNjIzMDQwNjMzLDgxNDA5NTk5NiwxMjM4NTQ2NzYs
LTEzMDU0MDE3ODMsLTM1NjQ0MjAzOCw0MjI1NTAyOV19
-->