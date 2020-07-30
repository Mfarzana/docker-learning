## Goals
- Pushing and Pulling to and from Docker Hub

 **Spring Boot Demo Project Structure:**
>![enter image description here](https://github.com/Mfarzana/docker-learning/blob/master/images/demo-project-structure.jpg)
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
  
## Pull docker image from Docker Hub Registry

**Step 1: Install Docker**
```
ubuntu@ip-172-31-4-174:~$ sudo apt update
ubuntu@ip-172-31-4-174:~$ sudo apt install docker.io
```
Step 2: pull image
```
ubuntu@ip-172-31-4-174:~$ sudo docker pull mfarzana/demo-spring:0.0.1
0.0.1: Pulling from mfarzana/demo-spring
```

## References
- https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
- https://medium.com/@wkrzywiec/how-to-put-your-java-application-into-docker-container-5e0a02acdd6b
- https://hub.docker.com/_/openjdk
- https://medium.com/@migueldoctor/how-to-create-a-custom-docker-image-with-jdk8-maven-and-gradle-ddc90f41cee4

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyMjE0NzAyMSwtMTgzMDkwNDAwLC0xMT
kxMTg5MTk2LDk5NzUzODgxMiwtMTk4MjkzMTk0MywxNjcwMzcx
NTcxLDExMzE4MjA0NzAsLTE3NDI3MDc1MDksMTIyNDcyOTI3Mi
wtMTE2MjQ1MDYwOCwtMjEyNzQ2MDM2MCwxNzE5MzYzNTg0LDE0
MzE5Njc4Miw5MDQzODIwNzUsLTU4MjkxNjI4NiwxMzc3MjMyMz
gwLDE3MDA4NTk3OTMsLTE2MjAwMTI0NDQsNjIwNzI5OTA2LDEz
NTExNjI3ODldfQ==
-->