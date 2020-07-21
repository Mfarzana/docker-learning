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
- provides a library interface to use system calls from programs
- Common task like opening,listing,reading and writing to files all involves system calls
- The fork() and exec() system calls determine how a process start
- **fork()**: the **kernel** **creates** an almost identical **copy of the current process and replaces that**
- exec(): the kernel starts a program,which replaces the current process
There is one more thing involved in this whole process called libraries which is just an additional code used by either shell or process to add more functionalities.For eg: the most important one is glibc which provides functions and system calls
### Init Process
Init is the parent of all Linux processes. It is the **first process** to start when a computer boots up and it runs until the system shuts down. It is the ancestor of all other processes.
### fork() System Call
System call fork() is used to create processes. It takes no arguments and returns a process ID. The purpose of fork() is to create a new process, which becomes the child process of the caller. After a new child process is created, both processes will execute the next instruction following the fork() system call. Therefore, we have to distinguish the parent from the child. This can be done by testing the returned value of fork():

## The underlying technology
### Namespaces
Docker uses a technology called  `namespaces`  to provide the isolated workspace called the  _container_. **When you run a container, Docker creates a set of  _namespaces_  for that container.**

These namespaces provide a layer of isolation. Each aspect of a container runs in a separate namespace and its access is limited to that namespace.

Docker Engine uses namespaces such as the following on Linux:

-   **The  `pid`  namespace:**  Process isolation (PID: Process ID).
-   **The  `net`  namespace:**  Managing network interfaces (NET: Networking).
-   **The  `ipc`  namespace:**  Managing access to IPC resources (IPC: InterProcess Communication).
-   **The  `mnt`  namespace:**  Managing filesystem mount points (MNT: Mount).
-   **The  `uts`  namespace:**  Isolating kernel and version identifiers. (UTS: Unix Timesharing System).
## Referece 
- https://docs.docker.com/get-started/overview/
 - https://medium.com/devops-world/how-linux-kernel-is-organized-56eafcace44
 - techopedia.com/definition/5306/network-interface-card-nic
 - https://community.fs.com/blog/nic-card-guide-for-beginners-functions-types-and-selection-tips.html
 - https://www.grandmetric.com/2018/01/20/how-does-routing-table-work/
 - https://www.ibm.com/support/knowledgecenter/en/ssw_aix_72/network/protocols_addr_resolution.html
 - https://unix.stackexchange.com/questions/87625/what-is-difference-between-user-space-and-kernel-space
 - https://www.redhat.com/en/blog/architecting-containers-part-1-why-understanding-user-space-vs-kernel-space-matters

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTY0OTU3MzE4MywtNDU2NzI2MTkwLDY4OD
E2ODU2NywtNTUwMzM2NjM1LDE2NTQ0NzIyOTcsNTQ0MjE5NTM0
LC05NTg5OTA3MDUsLTU2MjI1NjU5MSwtMTE3MzYzMzM1NCwtND
U4MzkwMjYsLTExMjAyOTIxNiwyMDk1ODE2MTE2LDE2MTU3Njg3
ODAsMjA4Mzc0NDUyNCwzODgxOTc3NjksLTE4NTAwMDQxNjYsND
k3ODE4ODEwLDczMDk5ODExNl19
-->