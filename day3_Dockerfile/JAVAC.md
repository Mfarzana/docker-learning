## Compile Java project in Docker container

**Step 1: Install Docker**
```
ubuntu@ip-172-31-4-174:~$ sudo apt update
ubuntu@ip-172-31-4-174:~$ sudo apt install docker.io
```
**Step 2: Create directory and write java code**
```
ubuntu@ip-172-31-4-174:~$ sudo mkdir javaapp
ubuntu@ip-172-31-4-174:~$ cd javaapp/
ubuntu@ip-172-31-4-174:~/javaapp$ sudo nano Main.java
```
**Java code** 
``` 
class Main{
    public static void main(String[] args) {
        System.out.println("Hello World "); 
    }
}
```

**Step 3: Write Dockerfile**
```
 ubuntu@ip-172-31-4-174:~/javaapp$ sudo nano Dockerfile
  ```

**Dockerfile**
```
FROM openjdk:8
COPY . /usr/src/myapp
WORKDIR usr/src/myapp
RUN javac Main.java
CMD ["java","Main"]
```
**Step 4: Build dockerfile**
```
ubuntu@ip-172-31-4-174:~/javaapp$ sudo docker build -t myapp:0.0.1 .
Sending build context to Docker daemon  3.072kB
...
Successfully built 2ca4b83d9050
Successfully tagged myapp:0.0.1
# list docker images
ubuntu@ip-172-31-4-174:~/javaapp$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
myapp               0.0.1               2ca4b83d9050        27 seconds ago      475MB
openjdk             7                   d735a2057e60        14 months ago       475MB
```
**Step 5: Run image**
```
ubuntu@ip-172-31-4-174:~/javaapp$ sudo docker run myapp:0.0.1
Hello World
```
## References
- https://hub.docker.com/_/openjdk


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ4MzU0NTU4Nyw0MDEwOTI3MDUsODUyNT
Y3MjM0LDE0NDc5NDAxNzQsLTQ5MjcxMDQ2NywtMTgxMzQ2Nzcz
MCwxNTQxNTQ4MTU4LDE2MTIwNjc1NzRdfQ==
-->
