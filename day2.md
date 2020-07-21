## Basic Concept of Docker Networking 
 ### NIC Card
A Network Interface Card (NIC) is a computer hardware component that allows a **computer to connect to a network**. NICs may be used for both wired and wireless connections.

### Routing table
**The place where routing information is stored is called a routing table.**  **Routing table contains** routing entries, that is list of **destinations** (often called: list of network prefixes or routes).
>  **Destination IP** field of packet is checked against **information stored in router**
>  
### Address Resolution Protocol (ARP)
 **ARP** dynamically **translates** Internet addresses (**IP addresses**/logical addresses) **into** the unique hardware addresses(**MAC addresses**/Physical addresses)on local area networks.
 
## Linux Kernel Concept 
**Linux OS has two-part**

 - **User Space**:**User**(Processes/**Application**/Services) need to do something and for that a typical interface is shell
 - **Kernel Space or Name Space**: Kernel is the only component that has direct access to hardware.

All processes make system calls:
![enter image description here](https://www.redhat.com/cms/managed-files/styles/wysiwyg_full_width/s3/2015/07/user-space-vs-kernel-space-simple-user-space.png?itok=7PGYkTdC)
 
 **Processes running** under the **user space** have access only to a limited part of memory, whereas the kernel has access to all of the memory. **Processes running in user space also don't have access to the kernel space.** **User space processes** can only **access** a small part of the **kernel** via an **interface** exposed by the kernel - **the system calls.**  If a process performs a system call, a software interrupt is sent to the kernel, which then dispatches the appropriate interrupt handler and continues its work after the handler has finished.
                      
### System Calls
-  Essential part of Linux Operating System
- Processes cannot access the kernel directly
- **System calls are used as an interface for processes to the kernel**. glibc
 provides a library interface to use system calls from programs
- Common task like opening,listing,reading and writing to files all involves system calls
- The fork() and exec() system calls determine how a process start
- **fork()**: the **kernel** **creates** an almost identical **copy of the current process and replaces that**
- exec(): the kernel starts a program,which replaces the current process
There is one more thing involved in this whole process called libraries which is just an additional code used by either shell or process to add more functionalities.

### Init Process
Init is the parent of all Linux processes. It is the **first process** to **start** when a **computer** boots up and it runs until the system shuts down. It is the ancestor of all other processes.

### Process Forking 
the way to **create a process** is to **create a copy of the existing process** and to go from there. This practice – known as **process forking** – involves duplicating the existing process to create a child process and making an exec system call to start another program.
> - System call fork() is used to create processes. 
> - When executed make two identical copies of address spaces, one for the parent and the other for the child.
	![enter image description here](https://www.oreilly.com/library/view/mastering-embedded-linux/9781787283282/assets/2dedcb0c-6bf9-4fca-90c4-f33790086eca.png)

### Daemons 
**Daemons are background process**.Some processes have the goal to run for a long time on the system in the background. This could be to fulfill requests like scanning an incoming email or sending back a page of a website. These processes are called daemons. **Daemons are often started directly after the operating system started.** Most have a ‘d’ at the end of the process name, to hint that they are a daemon process.
>- **Good to remember**: A daemon is always a process, but not all processes are a daemon
>- Daemons are spawned one of two ways**: either the init process forks and creates them directly 


## Docker technology
### Cgroups and Namespaces
Cgroups and namespaces are both **linux kernel features** that, together, create a way to **isolate a process or group of processes** to help create this abstraction we call a “**container**”. 

>- **Cgroups or control groups** are used to limit or monitor the resources of a group of processes.
>- **Namespaces** isolate what **a group of processes** have access to within the system. For example, a network namespace allows for different processes to use the same port without conflicting with one another. There is a process id namespace that could allow for multiple processes running PID 1. Or, perhaps, a mount namespace can isolate parts of the file system a group of processes have access to. In order to easily take advantage of these features together to create these abstract containers we need some sort of run-time.
>-   **The  `pid`  namespace:**  Process isolation (PID: Process ID).



## Referece 
- https://docs.docker.com/get-started/overview/
 - https://medium.com/devops-world/how-linux-kernel-is-organized-56eafcace44
 - https://www.techopedia.com/definition/5306/network-interface-card-nic
 - https://community.fs.com/blog/nic-card-guide-for-beginners-functions-types-and-selection-tips.html
 - https://www.grandmetric.com/2018/01/20/how-does-routing-table-work/
 - https://www.ibm.com/support/knowledgecenter/en/ssw_aix_72/network/protocols_addr_resolution.html
 - https://unix.stackexchange.com/questions/87625/what-is-difference-between-user-space-and-kernel-space
 - https://www.redhat.com/en/blog/architecting-containers-part-1-why-understanding-user-space-vs-kernel-space-matters
 - https://linux-audit.com/running-processes-and-daemons-on-linux-systems/
- https://thecodeboss.dev/2016/11/how-daemons-the-init-process-and-process-forking-work/
- https://www.shaunwarman.com/posts/docker-another-introduction.html
- 

<!--stackedit_data:
eyJoaXN0b3J5IjpbMzk5NDY0NzMzLDc5NTMzNDM5OSwxODgwNz
kzNDA3LC0zNDE4NTgwMTksLTI1OTIzNjUwMiwtMjU5MjM2NTAy
LDEyNjgxNDY1NjIsLTM1NTU4Mjc5NywtNTA3NDU4MzQsMTg2Mj
QzNzQzOCwxNTY3MDQ3Nzg4LC0zMjM3NTA5MjYsMjEwNDk1NDg4
NSwtMTQwODgyMjY0NywtMTE3ODk2MzQ1NSwtNDE0NjA3MDk2LC
00NTY3MjYxOTAsNjg4MTY4NTY3LC01NTAzMzY2MzUsMTY1NDQ3
MjI5N119
-->