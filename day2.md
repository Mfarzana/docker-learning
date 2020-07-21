## NIC Card
A Network Interface Card (NIC) is a computer hardware component that allows a **computer to connect to a network**. NICs may be used for both wired and wireless connections.
## Routing table
“destination IP field of packet is checked against information stored in router”. **The place where routing information is stored is called a routing table.**  Routing table contains routing entries, that is list of destinations (often called: list of network prefixes or routes).
## Linux 
**Linux OS has two-part**

 - **User Space**:**User**(Processes/**Application**/Services) need to do something and for that a typical interface is shell
 - **Kernel Space or Name Space**: Kernel is the only component that has direct access to hardware.

> User Space ----> Kernel Space
                     Signals
                     System Calls 
If **User need** to **interact** with **Kernel** there is a limited option,which is provided by kernel and strictly defined by the kernel what user can do
Signal
System Calls
## System Calls
-  Essential part of Linux Operating System
- Processes cannot access the kernel directly
- System calls are used as an interface for processes to the kernel. glibc
- provides a library interface to use system calls from programs
- Common task like opening,listing,reading and writing to files all involves system calls
- The fork() and exec() system calls determine how a process start
- fork(): the kernel creates an almost identical copy of the current process and replaces that
- exec(): the kernel starts a program,which replaces the current process
There is one more thing involved in this whole process called libraries which is just an additional code used by either shell or process to add more functionalities.For eg: the most important one is glibc which provides functions and system calls

## Referece 

 - https://medium.com/devops-world/how-linux-kernel-is-organized-56eafcace44
 - techopedia.com/definition/5306/network-interface-card-nic
 - https://community.fs.com/blog/nic-card-guide-for-beginners-functions-types-and-selection-tips.html
 - https://www.grandmetric.com/2018/01/20/how-does-routing-table-work/

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI3NjU3MTg4MSwtNDU4MzkwMjYsLTExMj
AyOTIxNiwyMDk1ODE2MTE2LDE2MTU3Njg3ODAsMjA4Mzc0NDUy
NCwzODgxOTc3NjksLTE4NTAwMDQxNjYsNDk3ODE4ODEwLDczMD
k5ODExNl19
-->