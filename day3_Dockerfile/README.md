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
Step 2: Pull image
```
ubuntu@ip-172-31-4-174:~$ sudo docker pull mfarzana/demo-spring:0.0.1
0.0.1: Pulling from mfarzana/demo-spring
```
Step 3: Run image
```
ubuntu@ip-172-31-4-174:~$ sudo docker run -p 8081:8082 mfarzana/demo-spring:0.0.1

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.3.1.RELEASE)

2020-07-30 07:33:36.990  INFO 1 --- [           main] com.example.demo.DemoApplication         : Starting DemoApplication v0.0.1-SNAPSHOT on db62a472e9fa with PID 1 (/usr/app/demo.war started by root in /usr/app)
2020-07-30 07:33:37.006  INFO 1 --- [           main] com.example.demo.DemoApplication         : No active profile set, falling back to default profiles: default
```

## References
- https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
- https://medium.com/@wkrzywiec/how-to-put-your-java-application-into-docker-container-5e0a02acdd6b
- https://hub.docker.com/_/openjdk
- https://medium.com/@migueldoctor/how-to-create-a-custom-docker-image-with-jdk8-maven-and-gradle-ddc90f41cee4

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTk1NDY3NTI1NCwtMTIyMTQ3MDIxLC0xOD
MwOTA0MDAsLTExOTExODkxOTYsOTk3NTM4ODEyLC0xOTgyOTMx
OTQzLDE2NzAzNzE1NzEsMTEzMTgyMDQ3MCwtMTc0MjcwNzUwOS
wxMjI0NzI5MjcyLC0xMTYyNDUwNjA4LC0yMTI3NDYwMzYwLDE3
MTkzNjM1ODQsMTQzMTk2NzgyLDkwNDM4MjA3NSwtNTgyOTE2Mj
g2LDEzNzcyMzIzODAsMTcwMDg1OTc5MywtMTYyMDAxMjQ0NCw2
MjA3Mjk5MDZdfQ==
-->