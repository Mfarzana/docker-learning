## Docker 
Docker is a platform for developers and sysadmins to develop, deploy, and run applications with containers. This is often described as containerization. 

## Containers vs. Virtual Machines
![enter image description here](https://www.itgratis.com/wp-content/uploads/2018/03/docker.jpg)

A **Dockerized application** is just a process that runs on your system. It **doesn’t require running a Hypervisor** (such as VMWare or VirtualBox), which means there’s **no guest operating system** to lug around. I do think there are reasons to use Virtual Machines nowadays, but they solve a different set of problems than Docker. You can use Docker to isolate individual applications, and use Virtual Machines to isolate entire systems. They are operating at different levels of abstraction.

## Docker Engine
Docker Engine is a **client-server application** with these major components:

- A **server** which is a type of long-running program called a **daemon process** (the dockerd command).

- A **REST API** which specifies interfaces that programs can use to talk to the daemon and instruct it what to do.

- A **command line interface** (CLI) **client** (the docker command).

![enter image description here](https://docs.docker.com/engine/images/engine-components-flow.png)

The **CLI uses** the **Docker REST API to control or interact** with the **Docker daemon** through scripting or direct CLI commands. The **daemon creates and manages Docker objects**, such as images, containers, networks, and volumes.

## Docker architecture
Docker uses a **client-server** architecture. The **Docker client talks to the Docker daemon,** which does the heavy lifting of **building, running, and distributing your Docker containers.** The **Docker client and daemon** can **run** on the s**ame system**, or you can connect a Docker client to a remote Docker daemon. The **Docker client ** and ** daemon communicate** using a **REST API**, over UNIX sockets or a network interface

![enter image description here](https://docs.docker.com/engine/images/architecture.svg)
  ### The Docker daemon
The Docker daemon (dockerd) **listens** for Docker **API requests** and **manages Docker objects** such as images, containers, networks, and volumes. A daemon can also communicate with other daemons to manage Docker services.
### The Docker client
The Docker client (docker) is the primary way that many **Docker users interact with Docker.** When you use commands such as docker run, the client sends these commands to dockerd, which carries them out. The docker command uses the Docker API. The Docker client can communicate with more than one daemon.
### Docker registries
A **Docker registry stores Docker images**. Docker Hub is a **public registry** that anyone can use, and Docker is configured to look for images on Docker Hub by default. You can even run your own **private registry.**
### IMAGES
Virtual machine environments, **images** would be called something **like snapshots**. They’re a picture of a Docker virtual machine at a specific point in time. Docker images are a little bit different from a virtual machine snapshot, though. For starters, Docker images can’t ever change. Once you’ve made one, you can delete it, but you can’t modify it. If you need a new version of the snapshot, you create an entirely new image
**NB: snapshot is a binary image of a filesystem**

### CONTAINERS
A container is a **runnable instance of an image.**

## Reference 
- https://docs.docker.com/get-started/overview/
- https://medium.com/codingthesmartway-com-blog/docker-beginners-guide-part-1-images-containers-6f3507fffc98#:~:text=Docker%20is%20a%20platform%20for,Docker%20containers%20are%20always%20portable.
- https://devopscon.io/blog/docker/docker-vs-virtual-machine-where-are-the-differences/
- https://blog.netapp.com/blogs/containers-vs-vms/amp/
- https://medium.com/better-programming/docker-containers-vs-virtual-machines-838022906016
- https://stackify.com/docker-image-vs-container-everything-you-need-to-know/
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg3NTg3MzQ3MiwtOTI5MTg3MDU0LDE2NT
M1OTExOCwtODg0MjMxMTAwLDE3NjUzMDIxMzYsNTUxOTM5ODQx
LDgzMjczOTk3MywtODExMDEzOTMxLDg0MjY4OTg3MiwxMDU2OD
E5MDA5LC0xMjM5NDg4MjIwLDYxOTQ1OTQxNCwtOTQ3MDI5Mjcw
LC0xMzA1Mjc2NjMyLDI2OTc1NDY5LC0yMDk5NzM5NDc0LC01MD
MzNDMxNzIsMjA3NzU4MTcxOCw3MzA5OTgxMTZdfQ==
-->