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
There is one more thing involved in this whole process called libraries which is just an additional code used by either shell or process to add more functionalities.For eg: the most important one is glibc which provides functions and system calls

### Init Process
Init is the parent of all Linux processes. It is the **first process** to start when a computer boots up and it runs until the system shuts down. It is the ancestor of all other processes.

### Process Forking 
the way to **create a process** is to **create a copy of the existing process** and to go from there. This practice – known as **process forking** – involves duplicating the existing process to create a child process and making an exec system call to start another program.
> - System call fork() is used to create processes. 
> - When executed make two identical copies of address spaces, one for the parent and the other for the child.
	![enter image description here](data:image/png;base64,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)

### Daemons 
**Daemons are background process**.Some processes have the goal to run for a long time on the system in the background. This could be to fulfill requests like scanning an incoming email or sending back a page of a website. These processes are called daemons. Besides the duration, another big difference is that daemons do not need interaction with the terminal. Typically they won’t send any data to it but use log files instead. Daemons are often started directly after the operating system started. Most have a ‘d’ at the end of the process name, to hint that they are a daemon process.



Good to remember: A daemon is always a process, but not all processes are a daemon

Daemons are spawned one of two ways: either the init process forks and creates them directly – like we mentioned above in the init process segment – or some other process will fork itself to create a child process, and then the parent process immediately exits. The first condition seems pretty straightforward – the init process forks to create a daemon – but how does that second condition work, and how does the init process end up becoming the parent of these daemons?

When you fork a process to create a child process, and then immediately kill that parent process, the child process becomes an orphaned process – a running process with no parent (not to be confused with a zombie process, such as a child process that has been terminated but is waiting on the parent process to read its exit status). By default, if a child process gets orphaned, the init process will automatically adopt the process and become its parent. This is a key concept to understand, because this is normally how daemons that you start after boot up relate to the init process. And that’s about all that makes daemons unique from normal background processes – see, not too bad!
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
eyJoaXN0b3J5IjpbLTEzOTUwODI4MCwtMTQwODgyMjY0NywtMT
E3ODk2MzQ1NSwtNDE0NjA3MDk2LC00NTY3MjYxOTAsNjg4MTY4
NTY3LC01NTAzMzY2MzUsMTY1NDQ3MjI5Nyw1NDQyMTk1MzQsLT
k1ODk5MDcwNSwtNTYyMjU2NTkxLC0xMTczNjMzMzU0LC00NTgz
OTAyNiwtMTEyMDI5MjE2LDIwOTU4MTYxMTYsMTYxNTc2ODc4MC
wyMDgzNzQ0NTI0LDM4ODE5Nzc2OSwtMTg1MDAwNDE2Niw0OTc4
MTg4MTBdfQ==
-->