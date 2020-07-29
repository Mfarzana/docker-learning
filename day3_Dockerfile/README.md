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
### Step 2: 
 
  


## Dockerfile
Docker builds images automatically by reading the instructions from a Dockerfile


## References
- https://docs.docker.com/develop/develop-images/dockerfile_best-practices/

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQ1OTY3MDc4NCwtMjA3MzgwMjMxNiwxMj
Q4NDA0OTgzLDYyMzA0MDYzMyw4MTQwOTU5OTYsMTIzODU0Njc2
LC0xMzA1NDAxNzgzLC0zNTY0NDIwMzgsNDIyNTUwMjldfQ==
-->