## Goals
- Create Dockerfile 
- Build a Docker image from Dockerfile
- Run image
- Pushing and Pulling to and from Docker Hub
## Run Java in docker container 
Step 1
```
ubuntu@ip-172-31-4-174:~$ sudo apt update
ubuntu@ip-172-31-4-174:~$ sudo apt install docker.io
ubuntu@ip-172-31-4-174:~$ pwd
/home/ubuntu

ubuntu@ip-172-31-4-174:~$ sudo mkdir javaapp
ubuntu@ip-172-31-4-174:~$ cd javaapp/
ubuntu@ip-172-31-4-174:~/javaapp$ sudo nano Main.java
ubuntu@ip-172-31-4-174:~/javaapp$ sudo nano Dockerfile
ubuntu@ip-172-31-4-174:~/javaapp$ sudo docker build -t myapp:0.0.1 .
Sending build context to Docker daemon  3.072kB
Step 1/5 : FROM openjdk:7
7: Pulling from library/openjdk
db0035920883: Pull complete
a9ebd83b4a47: Pull complete
4cf624e5b311: Pull complete
9acab6bfb3ef: Pull complete
0c00f0a5c1e2: Pull complete
98133370871a: Pull complete
ffd6078faaf1: Pull complete
Digest: sha256:75a05dbcd254fdde1a284c5cc47a8f7d5387cd517cbf9e66d50d45da1c695022
Status: Downloaded newer image for openjdk:7
 ---> d735a2057e60
Step 2/5 : COPY . /usr/src/myapp
 ---> 587e10061a77
Step 3/5 : WORKDIR usr/src/myapp
 ---> Running in 0b820c8c12ac
Removing intermediate container 0b820c8c12ac
 ---> 8fa7d5c89ca6
Step 4/5 : RUN javac Main.java
 ---> Running in b427053857c2
Removing intermediate container b427053857c2
 ---> 671b0e46af6c
Step 5/5 : CMD ["java","Main"]
 ---> Running in 7405bdf9073e
Removing intermediate container 7405bdf9073e
 ---> 2ca4b83d9050
Successfully built 2ca4b83d9050
Successfully tagged myapp:0.0.1
ubuntu@ip-172-31-4-174:~/javaapp$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
myapp               0.0.1               2ca4b83d9050        27 seconds ago      475MB
openjdk             7                   d735a2057e60        14 months ago       475MB
ubuntu@ip-172-31-4-174:~/javaapp$ sudo docker run myapp:0.0.1
Hello World


```
## Dockerfile
Dockerfile has two parts instruction and arguments. Docker builds images automatically by reading the instructions from a Dockerfile. 
- **FROM java:8-jdk-alpine** Our image will be based on another image that is available on public repository (Docker Hub) and **this image  contains all necessary dependencies** that we would need to run any **Java application**.

- **COPY ./target/demo.war /usr/app/**: First argument after COPY (/target/demo.war) is a **path of an application** that we want to put into container. The second parameter, /usr/app/ , is a **directory in a container** where we put the app.

- **WORKDIR /usr/app**: Use **/usr/app** folder as a **root**.
- **EXPOSE 8080** — Container will **listen** to **specific port**

- **ENTRYPOINT ["java", "-jar", "demo.war"]** —  Run the application, where first value is a command and the last two are parameters.

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
- https://medium.com/@migueldoctor/how-to-create-a-custom-docker-image-with-jdk8-maven-and-gradle-ddc90f41cee4

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTgxMjExNTA2NSwxMTMxODIwNDcwLC0xNz
QyNzA3NTA5LDEyMjQ3MjkyNzIsLTExNjI0NTA2MDgsLTIxMjc0
NjAzNjAsMTcxOTM2MzU4NCwxNDMxOTY3ODIsOTA0MzgyMDc1LC
01ODI5MTYyODYsMTM3NzIzMjM4MCwxNzAwODU5NzkzLC0xNjIw
MDEyNDQ0LDYyMDcyOTkwNiwxMzUxMTYyNzg5LDEwMzIxMTI3NT
MsLTExMDMwNzQ2NzcsLTc3MTcwNDM4OCwtMjA5NjMyMjgzNiwx
MzczMTAwNjU2XX0=
-->