> Written with [StackEdit](https://stackedit.io/).
## Docker 
Docker is a platform for developers and sysadmins to develop, deploy, and run applications with containers. This is often described as containerization. 

## Containers vs. Virtual Machines
![enter image description here](https://www.itgratis.com/wp-content/uploads/2018/03/docker.jpg)

Virtual machines and containers differ in several ways, but the primary difference is that **containers** provide a way to **virtualize an OS so that multiple workloads can run on a single OS** instance. With **VMs**, the hardware is being virtualized to **run multiple OS instances**. Containers’ speed, agility, and portability make them yet another tool to help streamline software development.

A **Dockerized application** is just a process that runs on your system. It **doesn’t require running a Hypervisor** (such as VMWare or VirtualBox), which means there’s **no guest operating system** to lug around. I do think there are reasons to use Virtual Machines nowadays, but they solve a different set of problems than Docker. You can use Docker to isolate individual applications, and use Virtual Machines to isolate entire systems. They are operating at different levels of abstraction.

## Docker Engine
Docker Engine is a **client-server application** with these major components:

- A **server** which is a type of long-running program called a **daemon process** (the dockerd command).

- A **REST API** which specifies interfaces that programs can use to talk to the daemon and instruct it what to do.

- A **command line interface** (CLI) **client** (the docker command).

![enter image description here](https://docs.docker.com/engine/images/engine-components-flow.png)

The **CLI uses** the **Docker REST API to control or interact** with the **Docker daemon** through scripting or direct CLI commands. The **daemon creates and manages Docker objects**, such as images, containers, networks, and volumes.

## Docker architecture
Docker uses a **client-server** architecture. The Docker client talks to the Docker daemon, which does the heavy lifting of building, running, and distributing your Docker containers. The Docker client and daemon can run on the same system, or you can connect a Docker client to a remote Docker daemon. The Docker client and daemon communicate using a REST API, over UNIX sockets or a network interface

![enter image description here](https://docs.docker.com/engine/images/architecture.svg)
## Reference 
- https://docs.docker.com/get-started/overview/
- https://medium.com/codingthesmartway-com-blog/docker-beginners-guide-part-1-images-containers-6f3507fffc98#:~:text=Docker%20is%20a%20platform%20for,Docker%20containers%20are%20always%20portable.
- https://devopscon.io/blog/docker/docker-vs-virtual-machine-where-are-the-differences/
- https://blog.netapp.com/blogs/containers-vs-vms/amp/
- https://medium.com/better-programming/docker-containers-vs-virtual-machines-838022906016
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzOTgzNTgxMjQsNTUxOTM5ODQxLDgzMj
czOTk3MywtODExMDEzOTMxLDg0MjY4OTg3MiwxMDU2ODE5MDA5
LC0xMjM5NDg4MjIwLDYxOTQ1OTQxNCwtOTQ3MDI5MjcwLC0xMz
A1Mjc2NjMyLDI2OTc1NDY5LC0yMDk5NzM5NDc0LC01MDMzNDMx
NzIsMjA3NzU4MTcxOCw3MzA5OTgxMTZdfQ==
-->