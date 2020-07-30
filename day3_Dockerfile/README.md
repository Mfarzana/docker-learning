## Goals
- Create Dockerfile 
- Build a Docker image from Dockerfile
- Run image
- Pushing and Pulling to and from Docker Hub
## Run Java in docker container 
```

ubuntu@ip-172-31-4-174:~$ sudo apt update
Hit:1 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal InRelease
Get:2 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal-updates InRelease [111 kB]
Get:3 http://security.ubuntu.com/ubuntu focal-security InRelease [107 kB]
Get:4 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal-backports InRelease [98.3 kB]
Get:5 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal/universe amd64 Packages [8628 kB]
Get:6 http://security.ubuntu.com/ubuntu focal-security/main amd64 Packages [148 kB]
Get:7 http://security.ubuntu.com/ubuntu focal-security/main Translation-en [52.0 kB]
Get:8 http://security.ubuntu.com/ubuntu focal-security/main amd64 c-n-f Metadata [3428 B]
Get:9 http://security.ubuntu.com/ubuntu focal-security/restricted amd64 Packages [29.2 kB]
Get:10 http://security.ubuntu.com/ubuntu focal-security/restricted Translation-en [7732 B]
Get:11 http://security.ubuntu.com/ubuntu focal-security/restricted amd64 c-n-f Metadata [324 B]
Get:12 http://security.ubuntu.com/ubuntu focal-security/universe amd64 Packages [44.4 kB]
Get:13 http://security.ubuntu.com/ubuntu focal-security/universe Translation-en [23.6 kB]
Get:14 http://security.ubuntu.com/ubuntu focal-security/universe amd64 c-n-f Metadata [1832 B]
Get:15 http://security.ubuntu.com/ubuntu focal-security/multiverse amd64 Packages [1172 B]
Get:16 http://security.ubuntu.com/ubuntu focal-security/multiverse Translation-en [540 B]
Get:17 http://security.ubuntu.com/ubuntu focal-security/multiverse amd64 c-n-f Metadata [116 B]
Get:18 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal/universe Translation-en [5124 kB]
Get:19 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal/universe amd64 c-n-f Metadata [265 kB]
Get:20 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal/multiverse amd64 Packages [144 kB]
Get:21 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal/multiverse Translation-en [104 kB]
Get:22 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal/multiverse amd64 c-n-f Metadata [9136 B]
Get:23 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal-updates/main amd64 Packages [314 kB]
Get:24 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal-updates/main Translation-en [118 kB]
Get:25 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal-updates/main amd64 c-n-f Metadata [7964 B]
Get:26 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal-updates/restricted amd64 Packages [29.2 kB]
Get:27 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal-updates/restricted Translation-en [7732 B]
Get:28 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal-updates/restricted amd64 c-n-f Metadata [324 B]
Get:29 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal-updates/universe amd64 Packages [146 kB]
Get:30 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal-updates/universe Translation-en [73.9 kB]
Get:31 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal-updates/universe amd64 c-n-f Metadata [4920 B]
Get:32 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal-updates/multiverse amd64 Packages [11.6 kB]
Get:33 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal-updates/multiverse Translation-en [3892 B]
Get:34 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal-updates/multiverse amd64 c-n-f Metadata [480 B]
Get:35 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal-backports/main amd64 c-n-f Metadata [112 B]
Get:36 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal-backports/restricted amd64 c-n-f Metadata [116 B]
Get:37 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal-backports/universe amd64 Packages [3096 B]
Get:38 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal-backports/universe Translation-en [1448 B]
Get:39 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal-backports/universe amd64 c-n-f Metadata [224 B]
Get:40 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal-backports/multiverse amd64 c-n-f Metadata [116 B]
Fetched 15.6 MB in 6s (2431 kB/s)
Reading package lists... Done
Building dependency tree
Reading state information... Done
82 packages can be upgraded. Run 'apt list --upgradable' to see them.
ubuntu@ip-172-31-4-174:~$ sudo apt install docker.io
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following additional packages will be installed:
  bridge-utils cgroupfs-mount containerd dns-root-data dnsmasq-base libidn11 pigz runc ubuntu-fan
Suggested packages:
  ifupdown aufs-tools debootstrap docker-doc rinse zfs-fuse | zfsutils
The following NEW packages will be installed:
  bridge-utils cgroupfs-mount containerd dns-root-data dnsmasq-base docker.io libidn11 pigz runc
  ubuntu-fan
0 upgraded, 10 newly installed, 0 to remove and 82 not upgraded.
Need to get 69.7 MB of archives.
After this operation, 334 MB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal/universe amd64 pigz amd64 2.4-1 [57.4 kB]
Get:2 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal/main amd64 bridge-utils amd64 1.6-2ubuntu1 [30.5 kB]
Get:3 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal/universe amd64 cgroupfs-mount all 1.4 [6320 B]
Get:4 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal/main amd64 runc amd64 1.0.0~rc10-0ubuntu1 [2549 kB]
Get:5 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal/main amd64 containerd amd64 1.3.3-0ubuntu2 [27.8 MB]
Get:6 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal/main amd64 dns-root-data all 2019052802 [5300 B]
Get:7 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal/main amd64 libidn11 amd64 1.33-2.2ubuntu2 [46.2 kB]
Get:8 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal/main amd64 dnsmasq-base amd64 2.80-1.1ubuntu1 [314 kB]
Get:9 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal-updates/universe amd64 docker.io amd64 19.03.8-0ubuntu1.20.04 [38.9 MB]
Get:10 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu focal/main amd64 ubuntu-fan all 0.12.13 [34.5 kB]
Fetched 69.7 MB in 17s (4118 kB/s)
Preconfiguring packages ...
Selecting previously unselected package pigz.
(Reading database ... 59609 files and directories currently installed.)
Preparing to unpack .../0-pigz_2.4-1_amd64.deb ...
Unpacking pigz (2.4-1) ...
Selecting previously unselected package bridge-utils.
Preparing to unpack .../1-bridge-utils_1.6-2ubuntu1_amd64.deb ...
Unpacking bridge-utils (1.6-2ubuntu1) ...
Selecting previously unselected package cgroupfs-mount.
Preparing to unpack .../2-cgroupfs-mount_1.4_all.deb ...
Unpacking cgroupfs-mount (1.4) ...
Selecting previously unselected package runc.
Preparing to unpack .../3-runc_1.0.0~rc10-0ubuntu1_amd64.deb ...
Unpacking runc (1.0.0~rc10-0ubuntu1) ...
Selecting previously unselected package containerd.
Preparing to unpack .../4-containerd_1.3.3-0ubuntu2_amd64.deb ...
Unpacking containerd (1.3.3-0ubuntu2) ...
Selecting previously unselected package dns-root-data.
Preparing to unpack .../5-dns-root-data_2019052802_all.deb ...
Unpacking dns-root-data (2019052802) ...
Selecting previously unselected package libidn11:amd64.
Preparing to unpack .../6-libidn11_1.33-2.2ubuntu2_amd64.deb ...
Unpacking libidn11:amd64 (1.33-2.2ubuntu2) ...
Selecting previously unselected package dnsmasq-base.
Preparing to unpack .../7-dnsmasq-base_2.80-1.1ubuntu1_amd64.deb ...
Unpacking dnsmasq-base (2.80-1.1ubuntu1) ...
Selecting previously unselected package docker.io.
Preparing to unpack .../8-docker.io_19.03.8-0ubuntu1.20.04_amd64.deb ...
Unpacking docker.io (19.03.8-0ubuntu1.20.04) ...
Selecting previously unselected package ubuntu-fan.
Preparing to unpack .../9-ubuntu-fan_0.12.13_all.deb ...
Unpacking ubuntu-fan (0.12.13) ...
Setting up runc (1.0.0~rc10-0ubuntu1) ...
Setting up dns-root-data (2019052802) ...
Setting up libidn11:amd64 (1.33-2.2ubuntu2) ...
Setting up bridge-utils (1.6-2ubuntu1) ...
Setting up pigz (2.4-1) ...
Setting up cgroupfs-mount (1.4) ...
Setting up containerd (1.3.3-0ubuntu2) ...
Created symlink /etc/systemd/system/multi-user.target.wants/containerd.service → /lib/systemd/system/containerd.service.
Setting up docker.io (19.03.8-0ubuntu1.20.04) ...
Adding group `docker' (GID 119) ...
Done.
Created symlink /etc/systemd/system/sockets.target.wants/docker.socket → /lib/systemd/system/docker.socket.
docker.service is a disabled or a static unit, not starting it.
Setting up dnsmasq-base (2.80-1.1ubuntu1) ...
Setting up ubuntu-fan (0.12.13) ...
Created symlink /etc/systemd/system/multi-user.target.wants/ubuntu-fan.service → /lib/systemd/system/ubuntu-fan.service.
Processing triggers for systemd (245.4-4ubuntu3.1) ...
Processing triggers for man-db (2.9.1-1) ...
Processing triggers for dbus (1.12.16-2ubuntu2) ...
Processing triggers for libc-bin (2.31-0ubuntu9) ...
ubuntu@ip-172-31-4-174:~$ mkdir
mkdir: missing operand
Try 'mkdir --help' for more information.
ubuntu@ip-172-31-4-174:~$ sudo mkdir
mkdir: missing operand
Try 'mkdir --help' for more information.
ubuntu@ip-172-31-4-174:~$ ls
ubuntu@ip-172-31-4-174:~$ pwd
/home/ubuntu
ubuntu@ip-172-31-4-174:~$ sudo /usr/src/
sudo: /usr/src/: command not found
ubuntu@ip-172-31-4-174:~$ sudo /usr/
bin/     include/ lib32/   libexec/ local/   share/
games/   lib/     lib64/   libx32/  sbin/    src/
ubuntu@ip-172-31-4-174:~$ sudo /usr/src/
sudo: /usr/src/: command not found
ubuntu@ip-172-31-4-174:~$ sudo cd /usr/src/
sudo: cd: command not found
ubuntu@ip-172-31-4-174:~$ cd /usr/src/
ubuntu@ip-172-31-4-174:/usr/src$ pwd
/usr/src
ubuntu@ip-172-31-4-174:/usr/src$ ls
linux-aws-headers-5.4.0-1015  linux-headers-5.4.0-1015-aws
ubuntu@ip-172-31-4-174:/usr/src$ cd ..
ubuntu@ip-172-31-4-174:/usr$ cd ..
ubuntu@ip-172-31-4-174:/$ cd ..
ubuntu@ip-172-31-4-174:/$ cd
ubuntu@ip-172-31-4-174:~$ pwd
/home/ubuntu
ubuntu@ip-172-31-4-174:~$ sudo mkdir javaapp
ubuntu@ip-172-31-4-174:~$ cd javaapp/
ubuntu@ip-172-31-4-174:~/javaapp$ sudo java nano Main.java
sudo: java: command not found
ubuntu@ip-172-31-4-174:~/javaapp$ sudo nano Main.java


Use "fg" to return to nano.

[1]+  Stopped                 sudo nano Main.java
ubuntu@ip-172-31-4-174:~/javaapp$ sudo nano Main.java
ubuntu@ip-172-31-4-174:~/javaapp$ sudo nano Main.java
ubuntu@ip-172-31-4-174:~/javaapp$ sudo nano Dockerfile
ubuntu@ip-172-31-4-174:~/javaapp$ docker build -t myapp:0.0.1 .
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post http://%2Fvar%2Frun%2Fdocker.sock/v1.40/build?buildargs=%7B%7D&cachefrom=%5B%5D&cgroupparent=&cpuperiod=0&cpuquota=0&cpusetcpus=&cpusetmems=&cpushares=0&dockerfile=Dockerfile&labels=%7B%7D&memory=0&memswap=0&networkmode=default&rm=1&session=vf67ogt6hm598yatydbzg29g4&shmsize=0&t=myapp%3A0.0.1&target=&ulimits=null&version=1: dial unix /var/run/docker.sock: connect: permission denied
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
ubuntu@ip-172-31-4-174:~/javaapp$ sudo images
sudo: images: command not found
ubuntu@ip-172-31-4-174:~/javaapp$ docker images
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.40/images/json: dial unix /var/run/docker.sock: connect: permission denied
ubuntu@ip-172-31-4-174:~/javaapp$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
myapp               0.0.1               2ca4b83d9050        27 seconds ago      475MB
openjdk             7                   d735a2057e60        14 months ago       475MB
ubuntu@ip-172-31-4-174:~/javaapp$ sudo docker run myapp:0.0.1
Hello World
ubuntu@ip-172-31-4-174:~/javaapp$


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
eyJoaXN0b3J5IjpbMTI1NTIxNzE5MywtMTc0MjcwNzUwOSwxMj
I0NzI5MjcyLC0xMTYyNDUwNjA4LC0yMTI3NDYwMzYwLDE3MTkz
NjM1ODQsMTQzMTk2NzgyLDkwNDM4MjA3NSwtNTgyOTE2Mjg2LD
EzNzcyMzIzODAsMTcwMDg1OTc5MywtMTYyMDAxMjQ0NCw2MjA3
Mjk5MDYsMTM1MTE2Mjc4OSwxMDMyMTEyNzUzLC0xMTAzMDc0Nj
c3LC03NzE3MDQzODgsLTIwOTYzMjI4MzYsMTM3MzEwMDY1Niwt
MjExNDE0NzcwMl19
-->