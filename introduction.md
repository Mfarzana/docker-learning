> Written with [StackEdit](https://stackedit.io/).
## Docker 
Docker is a platform for developers and sysadmins to develop, deploy, and run applications with containers. This is often described as containerization. 

## Containers vs. Virtual Machines
![enter image description here](https://www.itgratis.com/wp-content/uploads/2018/03/docker.jpg)

Virtual machines and containers differ in several ways, but the primary difference is that **containers** provide a way to **virtualize an OS so that multiple workloads can run on a single OS** instance. With **VMs**, the hardware is being virtualized to **run multiple OS instances**. Containers’ speed, agility, and portability make them yet another tool to help streamline software development.

A **Dockerized application** is just a process that runs on your system. It **doesn’t require running a Hypervisor** (such as VMWare or VirtualBox), which means there’s **no guest operating system** to lug around. I do think there are reasons to use Virtual Machines nowadays, but they solve a different set of problems than Docker. You can use Docker to isolate individual applications, and use Virtual Machines to isolate entire systems. They are operating at different levels of abstraction.
## Reference 
- https://docs.docker.com/get-started/overview/
- https://medium.com/codingthesmartway-com-blog/docker-beginners-guide-part-1-images-containers-6f3507fffc98#:~:text=Docker%20is%20a%20platform%20for,Docker%20containers%20are%20always%20portable.
- https://blog.netapp.com/blogs/containers-vs-vms/amp/
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwOTAxMjgzNzQsODQyNjg5ODcyLDEwNT
Y4MTkwMDksLTEyMzk0ODgyMjAsNjE5NDU5NDE0LC05NDcwMjky
NzAsLTEzMDUyNzY2MzIsMjY5NzU0NjksLTIwOTk3Mzk0NzQsLT
UwMzM0MzE3MiwyMDc3NTgxNzE4LDczMDk5ODExNl19
-->