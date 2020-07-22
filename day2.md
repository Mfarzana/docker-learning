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

## Practice：Creat two namesapcces and ping vice varsa
ubuntu@ip-172-31-10-25:~$ sudo ip netns add add ns1
ubuntu@ip-172-31-10-25:~$ sudo ip netns add ns2
ubuntu@ip-172-31-10-25:~$ sudo ip netns
ns2
add
white
black (id: 2)
blue (id: 1)
red (id: 0)
ubuntu@ip-172-31-10-25:~$ sudo ip netns add ns2
Cannot create namespace file "/run/netns/ns2": File exists
ubuntu@ip-172-31-10-25:~$ sudo ip netns add ns1
ubuntu@ip-172-31-10-25:~$ sudo ip netns
ns1
ns2
add
white
black (id: 2)
blue (id: 1)
red (id: 0)
ubuntu@ip-172-31-10-25:~$ sudo ip link add v-ns1 type veth peer name v-ns2
ubuntu@ip-172-31-10-25:~$ sudo ip link set v-ns1 netns ns1
ubuntu@ip-172-31-10-25:~$ sudo ip netns exec ns1 ip link
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
9: v-ns1@if8: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/ether 72:e1:ea:c4:f6:9a brd ff:ff:ff:ff:ff:ff link-netnsid 0
ubuntu@ip-172-31-10-25:~$ sudo ip link set v-ns2 netns ns2
ubuntu@ip-172-31-10-25:~$ sudo ip netns exec ns2 ip link
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
8: v-ns2@if9: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/ether f6:18:b3:c3:24:bc brd ff:ff:ff:ff:ff:ff link-netns ns1
ubuntu@ip-172-31-10-25:~$ sudo ip netns exec ns1 ip addr add 192.168.10.1/24 dev v-ns1
ubuntu@ip-172-31-10-25:~$ sudo ip netns exec ns1 ip link
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
9: v-ns1@if8: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/ether 72:e1:ea:c4:f6:9a brd ff:ff:ff:ff:ff:ff link-netns ns2
ubuntu@ip-172-31-10-25:~$ sudo ip netns exec ns2 ip addr add 192.168.10.2/24 dev v-ns2
ubuntu@ip-172-31-10-25:~$ sudo ip netns exec ns2
No command specified
ubuntu@ip-172-31-10-25:~$ sudo ip netns exec ns2 ip link
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
8: v-ns2@if9: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/ether f6:18:b3:c3:24:bc brd ff:ff:ff:ff:ff:ff link-netns ns1
ubuntu@ip-172-31-10-25:~$ sudo ip netns exec ns2 ip addr
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
8: v-ns2@if9: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether f6:18:b3:c3:24:bc brd ff:ff:ff:ff:ff:ff link-netns ns1
    inet 192.168.10.2/24 scope global v-ns2
       valid_lft forever preferred_lft forever
ubuntu@ip-172-31-10-25:~$ sudo ip netns ns1 ip link
Command "ns1" is unknown, try "ip netns help".
ubuntu@ip-172-31-10-25:~$ sudo ip netns exec ns1 ip link
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
9: v-ns1@if8: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/ether 72:e1:ea:c4:f6:9a brd ff:ff:ff:ff:ff:ff link-netns ns2
ubuntu@ip-172-31-10-25:~$ sudo ip netns exec ns1 ip link set lo up
ubuntu@ip-172-31-10-25:~$ sudo ip netns exec ns1 ip link set ^Cp
ubuntu@ip-172-31-10-25:~$ sudo ip netns exec ns1 ip link set v-ns1@if8 up
Cannot find device "v-ns1@if8"
ubuntu@ip-172-31-10-25:~$ sudo ip netns exec ns1 ip link set v-ns1 up
ubuntu@ip-172-31-10-25:~$ sudo ip netns exec ns2 ip link set v-ns2 up
ubuntu@ip-172-31-10-25:~$ sudo ip netns exec ns2 ip link set lo up
ubuntu@ip-172-31-10-25:~$ sudo ip netns exec ns1 ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
9: v-ns1@if8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 72:e1:ea:c4:f6:9a brd ff:ff:ff:ff:ff:ff link-netns ns2
    inet 192.168.10.1/24 scope global v-ns1
       valid_lft forever preferred_lft forever
    inet6 fe80::70e1:eaff:fec4:f69a/64 scope link
       valid_lft forever preferred_lft forever
ubuntu@ip-172-31-10-25:~$ sudo ip netns exec ns2 ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
8: v-ns2@if9: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether f6:18:b3:c3:24:bc brd ff:ff:ff:ff:ff:ff link-netns ns1
    inet 192.168.10.2/24 scope global v-ns2
       valid_lft forever preferred_lft forever
    inet6 fe80::f418:b3ff:fec3:24bc/64 scope link
       valid_lft forever preferred_lft forever
ubuntu@ip-172-31-10-25:~$ sudo ip netns ns1 ping 192.168.10.2
Command "ns1" is unknown, try "ip netns help".
ubuntu@ip-172-31-10-25:~$ sudo ip netns exec ns1 ping 192.168.10.2
PING 192.168.10.2 (192.168.10.2) 56(84) bytes of data.
64 bytes from 192.168.10.2: icmp_seq=1 ttl=64 time=0.037 ms
64 bytes from 192.168.10.2: icmp_seq=2 ttl=64 time=0.045 ms
64 bytes from 192.168.10.2: icmp_seq=3 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=4 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=5 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=6 ttl=64 time=0.087 ms
64 bytes from 192.168.10.2: icmp_seq=7 ttl=64 time=0.063 ms
64 bytes from 192.168.10.2: icmp_seq=8 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=9 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=10 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=11 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=12 ttl=64 time=0.062 ms
64 bytes from 192.168.10.2: icmp_seq=13 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=14 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=15 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=16 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=17 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=18 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=19 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=20 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=21 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=22 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=23 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=24 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=25 ttl=64 time=0.085 ms
64 bytes from 192.168.10.2: icmp_seq=26 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=27 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=28 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=29 ttl=64 time=0.046 ms
64 bytes from 192.168.10.2: icmp_seq=30 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=31 ttl=64 time=0.060 ms
n64 bytes from 192.168.10.2: icmp_seq=32 ttl=64 time=0.059 ms
w64 bytes from 192.168.10.2: icmp_seq=33 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=34 ttl=64 time=0.058 ms
64 bytes from 192.168.10.2: icmp_seq=35 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=36 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=37 ttl=64 time=0.046 ms
64 bytes from 192.168.10.2: icmp_seq=38 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=39 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=40 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=41 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=42 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=43 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=44 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=45 ttl=64 time=0.068 ms
64 bytes from 192.168.10.2: icmp_seq=46 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=47 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=48 ttl=64 time=0.053 ms
64 bytes from 192.168.10.2: icmp_seq=49 ttl=64 time=0.077 ms
64 bytes from 192.168.10.2: icmp_seq=50 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=51 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=52 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=53 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=54 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=55 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=56 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=57 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=58 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=59 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=60 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=61 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=62 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=63 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=64 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=65 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=66 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=67 ttl=64 time=0.046 ms
64 bytes from 192.168.10.2: icmp_seq=68 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=69 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=70 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=71 ttl=64 time=0.045 ms
64 bytes from 192.168.10.2: icmp_seq=72 ttl=64 time=0.072 ms
64 bytes from 192.168.10.2: icmp_seq=73 ttl=64 time=0.070 ms
64 bytes from 192.168.10.2: icmp_seq=74 ttl=64 time=0.059 ms
64 bytes from 192.168.10.2: icmp_seq=75 ttl=64 time=0.043 ms
64 bytes from 192.168.10.2: icmp_seq=76 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=77 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=78 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=79 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=80 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=81 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=82 ttl=64 time=0.063 ms
64 bytes from 192.168.10.2: icmp_seq=83 ttl=64 time=0.064 ms
64 bytes from 192.168.10.2: icmp_seq=84 ttl=64 time=0.062 ms
64 bytes from 192.168.10.2: icmp_seq=85 ttl=64 time=0.085 ms
64 bytes from 192.168.10.2: icmp_seq=86 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=87 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=88 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=89 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=90 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=91 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=92 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=93 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=94 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=95 ttl=64 time=0.046 ms
64 bytes from 192.168.10.2: icmp_seq=96 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=97 ttl=64 time=0.062 ms
64 bytes from 192.168.10.2: icmp_seq=98 ttl=64 time=0.046 ms
64 bytes from 192.168.10.2: icmp_seq=99 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=100 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=101 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=102 ttl=64 time=0.046 ms
64 bytes from 192.168.10.2: icmp_seq=103 ttl=64 time=0.053 ms
64 bytes from 192.168.10.2: icmp_seq=104 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=105 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=106 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=107 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=108 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=109 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=110 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=111 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=112 ttl=64 time=0.063 ms
64 bytes from 192.168.10.2: icmp_seq=113 ttl=64 time=0.046 ms
64 bytes from 192.168.10.2: icmp_seq=114 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=115 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=116 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=117 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=118 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=119 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=120 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=121 ttl=64 time=0.056 ms
64 bytes from 192.168.10.2: icmp_seq=122 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=123 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=124 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=125 ttl=64 time=0.063 ms
64 bytes from 192.168.10.2: icmp_seq=126 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=127 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=128 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=129 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=130 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=131 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=132 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=133 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=134 ttl=64 time=0.046 ms
64 bytes from 192.168.10.2: icmp_seq=135 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=136 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=137 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=138 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=139 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=140 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=141 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=142 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=143 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=144 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=145 ttl=64 time=0.075 ms
64 bytes from 192.168.10.2: icmp_seq=146 ttl=64 time=0.071 ms
64 bytes from 192.168.10.2: icmp_seq=147 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=148 ttl=64 time=0.059 ms
64 bytes from 192.168.10.2: icmp_seq=149 ttl=64 time=0.063 ms
64 bytes from 192.168.10.2: icmp_seq=150 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=151 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=152 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=153 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=154 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=155 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=156 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=157 ttl=64 time=0.058 ms
64 bytes from 192.168.10.2: icmp_seq=158 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=159 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=160 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=161 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=162 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=163 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=164 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=165 ttl=64 time=0.072 ms
64 bytes from 192.168.10.2: icmp_seq=166 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=167 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=168 ttl=64 time=0.055 ms
64 bytes from 192.168.10.2: icmp_seq=169 ttl=64 time=0.072 ms
64 bytes from 192.168.10.2: icmp_seq=170 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=171 ttl=64 time=0.057 ms
64 bytes from 192.168.10.2: icmp_seq=172 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=173 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=174 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=175 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=176 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=177 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=178 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=179 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=180 ttl=64 time=0.058 ms
64 bytes from 192.168.10.2: icmp_seq=181 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=182 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=183 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=184 ttl=64 time=0.054 ms
64 bytes from 192.168.10.2: icmp_seq=185 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=186 ttl=64 time=0.054 ms
64 bytes from 192.168.10.2: icmp_seq=187 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=188 ttl=64 time=0.056 ms
64 bytes from 192.168.10.2: icmp_seq=189 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=190 ttl=64 time=0.062 ms
64 bytes from 192.168.10.2: icmp_seq=191 ttl=64 time=0.064 ms
64 bytes from 192.168.10.2: icmp_seq=192 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=193 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=194 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=195 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=196 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=197 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=198 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=199 ttl=64 time=0.059 ms
64 bytes from 192.168.10.2: icmp_seq=200 ttl=64 time=0.054 ms
64 bytes from 192.168.10.2: icmp_seq=201 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=202 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=203 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=204 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=205 ttl=64 time=0.079 ms
64 bytes from 192.168.10.2: icmp_seq=206 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=207 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=208 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=209 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=210 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=211 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=212 ttl=64 time=0.068 ms
64 bytes from 192.168.10.2: icmp_seq=213 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=214 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=215 ttl=64 time=0.053 ms
64 bytes from 192.168.10.2: icmp_seq=216 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=217 ttl=64 time=0.064 ms
64 bytes from 192.168.10.2: icmp_seq=218 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=219 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=220 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=221 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=222 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=223 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=224 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=225 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=226 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=227 ttl=64 time=0.046 ms
64 bytes from 192.168.10.2: icmp_seq=228 ttl=64 time=0.045 ms
64 bytes from 192.168.10.2: icmp_seq=229 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=230 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=231 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=232 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=233 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=234 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=235 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=236 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=237 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=238 ttl=64 time=0.053 ms
64 bytes from 192.168.10.2: icmp_seq=239 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=240 ttl=64 time=0.054 ms
64 bytes from 192.168.10.2: icmp_seq=241 ttl=64 time=0.062 ms
64 bytes from 192.168.10.2: icmp_seq=242 ttl=64 time=0.028 ms
64 bytes from 192.168.10.2: icmp_seq=243 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=244 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=245 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=246 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=247 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=248 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=249 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=250 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=251 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=252 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=253 ttl=64 time=0.058 ms
64 bytes from 192.168.10.2: icmp_seq=254 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=255 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=256 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=257 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=258 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=259 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=260 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=261 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=262 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=263 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=264 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=265 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=266 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=267 ttl=64 time=0.046 ms
64 bytes from 192.168.10.2: icmp_seq=268 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=269 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=270 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=271 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=272 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=273 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=274 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=275 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=276 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=277 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=278 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=279 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=280 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=281 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=282 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=283 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=284 ttl=64 time=0.054 ms
64 bytes from 192.168.10.2: icmp_seq=285 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=286 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=287 ttl=64 time=0.074 ms
64 bytes from 192.168.10.2: icmp_seq=288 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=289 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=290 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=291 ttl=64 time=0.046 ms
64 bytes from 192.168.10.2: icmp_seq=292 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=293 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=294 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=295 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=296 ttl=64 time=0.053 ms
64 bytes from 192.168.10.2: icmp_seq=297 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=298 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=299 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=300 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=301 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=302 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=303 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=304 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=305 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=306 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=307 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=308 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=309 ttl=64 time=0.076 ms
64 bytes from 192.168.10.2: icmp_seq=310 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=311 ttl=64 time=0.046 ms
64 bytes from 192.168.10.2: icmp_seq=312 ttl=64 time=0.045 ms
64 bytes from 192.168.10.2: icmp_seq=313 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=314 ttl=64 time=0.046 ms
64 bytes from 192.168.10.2: icmp_seq=315 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=316 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=317 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=318 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=319 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=320 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=321 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=322 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=323 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=324 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=325 ttl=64 time=0.058 ms
64 bytes from 192.168.10.2: icmp_seq=326 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=327 ttl=64 time=0.053 ms
64 bytes from 192.168.10.2: icmp_seq=328 ttl=64 time=0.064 ms
64 bytes from 192.168.10.2: icmp_seq=329 ttl=64 time=0.067 ms
64 bytes from 192.168.10.2: icmp_seq=330 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=331 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=332 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=333 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=334 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=335 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=336 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=337 ttl=64 time=0.068 ms
64 bytes from 192.168.10.2: icmp_seq=338 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=339 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=340 ttl=64 time=0.043 ms
64 bytes from 192.168.10.2: icmp_seq=341 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=342 ttl=64 time=0.046 ms
64 bytes from 192.168.10.2: icmp_seq=343 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=344 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=345 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=346 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=347 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=348 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=349 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=350 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=351 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=352 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=353 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=354 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=355 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=356 ttl=64 time=0.046 ms
64 bytes from 192.168.10.2: icmp_seq=357 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=358 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=359 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=360 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=361 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=362 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=363 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=364 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=365 ttl=64 time=0.053 ms
64 bytes from 192.168.10.2: icmp_seq=366 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=367 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=368 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=369 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=370 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=371 ttl=64 time=0.074 ms
64 bytes from 192.168.10.2: icmp_seq=372 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=373 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=374 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=375 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=376 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=377 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=378 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=379 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=380 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=381 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=382 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=383 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=384 ttl=64 time=0.053 ms
64 bytes from 192.168.10.2: icmp_seq=385 ttl=64 time=0.063 ms
64 bytes from 192.168.10.2: icmp_seq=386 ttl=64 time=0.054 ms
64 bytes from 192.168.10.2: icmp_seq=387 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=388 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=389 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=390 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=391 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=392 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=393 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=394 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=395 ttl=64 time=0.055 ms
64 bytes from 192.168.10.2: icmp_seq=396 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=397 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=398 ttl=64 time=0.053 ms
64 bytes from 192.168.10.2: icmp_seq=399 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=400 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=401 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=402 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=403 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=404 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=405 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=406 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=407 ttl=64 time=0.059 ms
64 bytes from 192.168.10.2: icmp_seq=408 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=409 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=410 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=411 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=412 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=413 ttl=64 time=0.071 ms
64 bytes from 192.168.10.2: icmp_seq=414 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=415 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=416 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=417 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=418 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=419 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=420 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=421 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=422 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=423 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=424 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=425 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=426 ttl=64 time=0.053 ms
64 bytes from 192.168.10.2: icmp_seq=427 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=428 ttl=64 time=0.053 ms
64 bytes from 192.168.10.2: icmp_seq=429 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=430 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=431 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=432 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=433 ttl=64 time=0.059 ms
64 bytes from 192.168.10.2: icmp_seq=434 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=435 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=436 ttl=64 time=0.046 ms
64 bytes from 192.168.10.2: icmp_seq=437 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=438 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=439 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=440 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=441 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=442 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=443 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=444 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=445 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=446 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=447 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=448 ttl=64 time=0.046 ms
64 bytes from 192.168.10.2: icmp_seq=449 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=450 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=451 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=452 ttl=64 time=0.055 ms
64 bytes from 192.168.10.2: icmp_seq=453 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=454 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=455 ttl=64 time=0.068 ms
64 bytes from 192.168.10.2: icmp_seq=456 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=457 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=458 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=459 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=460 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=461 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=462 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=463 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=464 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=465 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=466 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=467 ttl=64 time=0.053 ms
64 bytes from 192.168.10.2: icmp_seq=468 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=469 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=470 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=471 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=472 ttl=64 time=0.056 ms
64 bytes from 192.168.10.2: icmp_seq=473 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=474 ttl=64 time=0.054 ms
64 bytes from 192.168.10.2: icmp_seq=475 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=476 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=477 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=478 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=479 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=480 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=481 ttl=64 time=0.065 ms
64 bytes from 192.168.10.2: icmp_seq=482 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=483 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=484 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=485 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=486 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=487 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=488 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=489 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=490 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=491 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=492 ttl=64 time=0.053 ms
64 bytes from 192.168.10.2: icmp_seq=493 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=494 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=495 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=496 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=497 ttl=64 time=0.064 ms
64 bytes from 192.168.10.2: icmp_seq=498 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=499 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=500 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=501 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=502 ttl=64 time=0.046 ms
64 bytes from 192.168.10.2: icmp_seq=503 ttl=64 time=0.058 ms
64 bytes from 192.168.10.2: icmp_seq=504 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=505 ttl=64 time=0.056 ms
64 bytes from 192.168.10.2: icmp_seq=506 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=507 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=508 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=509 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=510 ttl=64 time=0.058 ms
64 bytes from 192.168.10.2: icmp_seq=511 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=512 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=513 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=514 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=515 ttl=64 time=0.054 ms
64 bytes from 192.168.10.2: icmp_seq=516 ttl=64 time=0.046 ms
64 bytes from 192.168.10.2: icmp_seq=517 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=518 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=519 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=520 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=521 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=522 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=523 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=524 ttl=64 time=0.062 ms
64 bytes from 192.168.10.2: icmp_seq=525 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=526 ttl=64 time=0.062 ms
64 bytes from 192.168.10.2: icmp_seq=527 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=528 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=529 ttl=64 time=0.072 ms
64 bytes from 192.168.10.2: icmp_seq=530 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=531 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=532 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=533 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=534 ttl=64 time=0.072 ms
64 bytes from 192.168.10.2: icmp_seq=535 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=536 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=537 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=538 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=539 ttl=64 time=0.065 ms
64 bytes from 192.168.10.2: icmp_seq=540 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=541 ttl=64 time=0.072 ms
64 bytes from 192.168.10.2: icmp_seq=542 ttl=64 time=0.059 ms
64 bytes from 192.168.10.2: icmp_seq=543 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=544 ttl=64 time=0.046 ms
64 bytes from 192.168.10.2: icmp_seq=545 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=546 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=547 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=548 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=549 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=550 ttl=64 time=0.068 ms
64 bytes from 192.168.10.2: icmp_seq=551 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=552 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=553 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=554 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=555 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=556 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=557 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=558 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=559 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=560 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=561 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=562 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=563 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=564 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=565 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=566 ttl=64 time=0.065 ms
64 bytes from 192.168.10.2: icmp_seq=567 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=568 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=569 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=570 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=571 ttl=64 time=0.070 ms
64 bytes from 192.168.10.2: icmp_seq=572 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=573 ttl=64 time=0.053 ms
64 bytes from 192.168.10.2: icmp_seq=574 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=575 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=576 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=577 ttl=64 time=0.059 ms
64 bytes from 192.168.10.2: icmp_seq=578 ttl=64 time=0.063 ms
64 bytes from 192.168.10.2: icmp_seq=579 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=580 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=581 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=582 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=583 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=584 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=585 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=586 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=587 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=588 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=589 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=590 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=591 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=592 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=593 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=594 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=595 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=596 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=597 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=598 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=599 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=600 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=601 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=602 ttl=64 time=0.064 ms
64 bytes from 192.168.10.2: icmp_seq=603 ttl=64 time=0.083 ms
64 bytes from 192.168.10.2: icmp_seq=604 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=605 ttl=64 time=0.059 ms
64 bytes from 192.168.10.2: icmp_seq=606 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=607 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=608 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=609 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=610 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=611 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=612 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=613 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=614 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=615 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=616 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=617 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=618 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=619 ttl=64 time=0.065 ms
64 bytes from 192.168.10.2: icmp_seq=620 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=621 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=622 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=623 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=624 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=625 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=626 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=627 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=628 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=629 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=630 ttl=64 time=0.054 ms
64 bytes from 192.168.10.2: icmp_seq=631 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=632 ttl=64 time=0.058 ms
64 bytes from 192.168.10.2: icmp_seq=633 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=634 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=635 ttl=64 time=0.058 ms
64 bytes from 192.168.10.2: icmp_seq=636 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=637 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=638 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=639 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=640 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=641 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=642 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=643 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=644 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=645 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=646 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=647 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=648 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=649 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=650 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=651 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=652 ttl=64 time=0.046 ms
64 bytes from 192.168.10.2: icmp_seq=653 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=654 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=655 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=656 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=657 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=658 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=659 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=660 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=661 ttl=64 time=0.058 ms
64 bytes from 192.168.10.2: icmp_seq=662 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=663 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=664 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=665 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=666 ttl=64 time=0.056 ms
64 bytes from 192.168.10.2: icmp_seq=667 ttl=64 time=0.056 ms
64 bytes from 192.168.10.2: icmp_seq=668 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=669 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=670 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=671 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=672 ttl=64 time=0.064 ms
64 bytes from 192.168.10.2: icmp_seq=673 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=674 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=675 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=676 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=677 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=678 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=679 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=680 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=681 ttl=64 time=0.066 ms
64 bytes from 192.168.10.2: icmp_seq=682 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=683 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=684 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=685 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=686 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=687 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=688 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=689 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=690 ttl=64 time=0.055 ms
64 bytes from 192.168.10.2: icmp_seq=691 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=692 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=693 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=694 ttl=64 time=0.058 ms
64 bytes from 192.168.10.2: icmp_seq=695 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=696 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=697 ttl=64 time=0.053 ms
64 bytes from 192.168.10.2: icmp_seq=698 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=699 ttl=64 time=0.062 ms
64 bytes from 192.168.10.2: icmp_seq=700 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=701 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=702 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=703 ttl=64 time=0.097 ms
64 bytes from 192.168.10.2: icmp_seq=704 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=705 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=706 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=707 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=708 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=709 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=710 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=711 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=712 ttl=64 time=0.057 ms
64 bytes from 192.168.10.2: icmp_seq=713 ttl=64 time=0.055 ms
64 bytes from 192.168.10.2: icmp_seq=714 ttl=64 time=0.053 ms
64 bytes from 192.168.10.2: icmp_seq=715 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=716 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=717 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=718 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=719 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=720 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=721 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=722 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=723 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=724 ttl=64 time=0.053 ms
64 bytes from 192.168.10.2: icmp_seq=725 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=726 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=727 ttl=64 time=0.053 ms
64 bytes from 192.168.10.2: icmp_seq=728 ttl=64 time=0.053 ms
64 bytes from 192.168.10.2: icmp_seq=729 ttl=64 time=0.063 ms
64 bytes from 192.168.10.2: icmp_seq=730 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=731 ttl=64 time=0.056 ms
64 bytes from 192.168.10.2: icmp_seq=732 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=733 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=734 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=735 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=736 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=737 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=738 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=739 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=740 ttl=64 time=0.063 ms
64 bytes from 192.168.10.2: icmp_seq=741 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=742 ttl=64 time=0.063 ms
64 bytes from 192.168.10.2: icmp_seq=743 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=744 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=745 ttl=64 time=0.063 ms
64 bytes from 192.168.10.2: icmp_seq=746 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=747 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=748 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=749 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=750 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=751 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=752 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=753 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=754 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=755 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=756 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=757 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=758 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=759 ttl=64 time=0.062 ms
64 bytes from 192.168.10.2: icmp_seq=760 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=761 ttl=64 time=0.062 ms
64 bytes from 192.168.10.2: icmp_seq=762 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=763 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=764 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=765 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=766 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=767 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=768 ttl=64 time=0.059 ms
64 bytes from 192.168.10.2: icmp_seq=769 ttl=64 time=0.062 ms
64 bytes from 192.168.10.2: icmp_seq=770 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=771 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=772 ttl=64 time=0.063 ms
64 bytes from 192.168.10.2: icmp_seq=773 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=774 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=775 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=776 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=777 ttl=64 time=0.071 ms
64 bytes from 192.168.10.2: icmp_seq=778 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=779 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=780 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=781 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=782 ttl=64 time=0.053 ms
64 bytes from 192.168.10.2: icmp_seq=783 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=784 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=785 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=786 ttl=64 time=0.063 ms
64 bytes from 192.168.10.2: icmp_seq=787 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=788 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=789 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=790 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=791 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=792 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=793 ttl=64 time=0.059 ms
64 bytes from 192.168.10.2: icmp_seq=794 ttl=64 time=0.056 ms
64 bytes from 192.168.10.2: icmp_seq=795 ttl=64 time=0.067 ms
64 bytes from 192.168.10.2: icmp_seq=796 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=797 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=798 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=799 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=800 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=801 ttl=64 time=0.054 ms
64 bytes from 192.168.10.2: icmp_seq=802 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=803 ttl=64 time=0.058 ms
64 bytes from 192.168.10.2: icmp_seq=804 ttl=64 time=0.062 ms
64 bytes from 192.168.10.2: icmp_seq=805 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=806 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=807 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=808 ttl=64 time=0.059 ms
64 bytes from 192.168.10.2: icmp_seq=809 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=810 ttl=64 time=0.062 ms
64 bytes from 192.168.10.2: icmp_seq=811 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=812 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=813 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=814 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=815 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=816 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=817 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=818 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=819 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=820 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=821 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=822 ttl=64 time=0.067 ms
64 bytes from 192.168.10.2: icmp_seq=823 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=824 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=825 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=826 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=827 ttl=64 time=0.069 ms
64 bytes from 192.168.10.2: icmp_seq=828 ttl=64 time=0.053 ms
64 bytes from 192.168.10.2: icmp_seq=829 ttl=64 time=0.077 ms
64 bytes from 192.168.10.2: icmp_seq=830 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=831 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=832 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=833 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=834 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=835 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=836 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=837 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=838 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=839 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=840 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=841 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=842 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=843 ttl=64 time=0.059 ms
64 bytes from 192.168.10.2: icmp_seq=844 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=845 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=846 ttl=64 time=0.053 ms
64 bytes from 192.168.10.2: icmp_seq=847 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=848 ttl=64 time=0.058 ms
64 bytes from 192.168.10.2: icmp_seq=849 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=850 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=851 ttl=64 time=0.053 ms
64 bytes from 192.168.10.2: icmp_seq=852 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=853 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=854 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=855 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=856 ttl=64 time=0.068 ms
64 bytes from 192.168.10.2: icmp_seq=857 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=858 ttl=64 time=0.063 ms
64 bytes from 192.168.10.2: icmp_seq=859 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=860 ttl=64 time=0.041 ms
64 bytes from 192.168.10.2: icmp_seq=861 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=862 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=863 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=864 ttl=64 time=0.072 ms
64 bytes from 192.168.10.2: icmp_seq=865 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=866 ttl=64 time=0.064 ms
64 bytes from 192.168.10.2: icmp_seq=867 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=868 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=869 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=870 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=871 ttl=64 time=0.065 ms
64 bytes from 192.168.10.2: icmp_seq=872 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=873 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=874 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=875 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=876 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=877 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=878 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=879 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=880 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=881 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=882 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=883 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=884 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=885 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=886 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=887 ttl=64 time=0.073 ms
64 bytes from 192.168.10.2: icmp_seq=888 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=889 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=890 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=891 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=892 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=893 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=894 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=895 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=896 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=897 ttl=64 time=0.073 ms
64 bytes from 192.168.10.2: icmp_seq=898 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=899 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=900 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=901 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=902 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=903 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=904 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=905 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=906 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=907 ttl=64 time=0.043 ms
64 bytes from 192.168.10.2: icmp_seq=908 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=909 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=910 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=911 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=912 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=913 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=914 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=915 ttl=64 time=0.059 ms
64 bytes from 192.168.10.2: icmp_seq=916 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=917 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=918 ttl=64 time=0.064 ms
64 bytes from 192.168.10.2: icmp_seq=919 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=920 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=921 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=922 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=923 ttl=64 time=0.063 ms
64 bytes from 192.168.10.2: icmp_seq=924 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=925 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=926 ttl=64 time=0.064 ms
64 bytes from 192.168.10.2: icmp_seq=927 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=928 ttl=64 time=0.052 ms
64 bytes from 192.168.10.2: icmp_seq=929 ttl=64 time=0.063 ms
64 bytes from 192.168.10.2: icmp_seq=930 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=931 ttl=64 time=0.062 ms
64 bytes from 192.168.10.2: icmp_seq=932 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=933 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=934 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=935 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=936 ttl=64 time=0.073 ms
64 bytes from 192.168.10.2: icmp_seq=937 ttl=64 time=0.071 ms
64 bytes from 192.168.10.2: icmp_seq=938 ttl=64 time=0.064 ms
64 bytes from 192.168.10.2: icmp_seq=939 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=940 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=941 ttl=64 time=0.064 ms
64 bytes from 192.168.10.2: icmp_seq=942 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=943 ttl=64 time=0.070 ms
64 bytes from 192.168.10.2: icmp_seq=944 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=945 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=946 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=947 ttl=64 time=0.062 ms
64 bytes from 192.168.10.2: icmp_seq=948 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=949 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=950 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=951 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=952 ttl=64 time=0.060 ms
64 bytes from 192.168.10.2: icmp_seq=953 ttl=64 time=0.046 ms
64 bytes from 192.168.10.2: icmp_seq=954 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=955 ttl=64 time=0.046 ms
64 bytes from 192.168.10.2: icmp_seq=956 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=957 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=958 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=959 ttl=64 time=0.048 ms
64 bytes from 192.168.10.2: icmp_seq=960 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=961 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=962 ttl=64 time=0.059 ms
64 bytes from 192.168.10.2: icmp_seq=963 ttl=64 time=0.047 ms
64 bytes from 192.168.10.2: icmp_seq=964 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=965 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=966 ttl=64 time=0.064 ms
64 bytes from 192.168.10.2: icmp_seq=967 ttl=64 time=0.050 ms
64 bytes from 192.168.10.2: icmp_seq=968 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=969 ttl=64 time=0.049 ms
64 bytes from 192.168.10.2: icmp_seq=970 ttl=64 time=0.051 ms
64 bytes from 192.168.10.2: icmp_seq=971 ttl=64 time=0.061 ms
64 bytes from 192.168.10.2: icmp_seq=972 ttl=64 time=0.051 ms
^C
--- 192.168.10.2 ping statistics ---
972 packets transmitted, 972 received, 0% packet loss, time 994295ms
rtt min/avg/max/mdev = 0.028/0.052/0.097/0.006 ms
ubuntu@ip-172-31-10-25:~$ sudo ip netns exec ns2 ping 192.168.10.1
PING 192.168.10.1 (192.168.10.1) 56(84) bytes of data.
64 bytes from 192.168.10.1: icmp_seq=1 ttl=64 time=0.022 ms
64 bytes from 192.168.10.1: icmp_seq=2 ttl=64 time=0.047 ms
64 bytes from 192.168.10.1: icmp_seq=3 ttl=64 time=0.048 ms
64 bytes from 192.168.10.1: icmp_seq=4 ttl=64 time=0.050 ms
64 bytes from 192.168.10.1: icmp_seq=5 ttl=64 time=0.052 ms
64 bytes from 192.168.10.1: icmp_seq=6 ttl=64 time=0.071 ms
64 bytes from 192.168.10.1: icmp_seq=7 ttl=64 time=0.052 ms
64 bytes from 192.168.10.1: icmp_seq=8 ttl=64 time=0.050 ms
64 bytes from 192.168.10.1: icmp_seq=9 ttl=64 time=0.047 ms
64 bytes from 192.168.10.1: icmp_seq=10 ttl=64 time=0.048 ms
^C
--- 192.168.10.1 ping statistics ---
10 packets transmitted, 10 received, 0% packet loss, time 9206ms
rtt min/avg/max/mdev = 0.022/0.048/0.071/0.011 ms
ubuntu@ip-172-31-10-25:~$ sudo ip netns exec ns2 ping ns1
ping: ns1: Temporary failure in name resolution
ubuntu@ip-172-31-10-25:~$


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

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTc4Mzg1NjUxLC0xNDcwMTg2Mzk4LDM5OT
Q2NDczMyw3OTUzMzQzOTksMTg4MDc5MzQwNywtMzQxODU4MDE5
LC0yNTkyMzY1MDIsLTI1OTIzNjUwMiwxMjY4MTQ2NTYyLC0zNT
U1ODI3OTcsLTUwNzQ1ODM0LDE4NjI0Mzc0MzgsMTU2NzA0Nzc4
OCwtMzIzNzUwOTI2LDIxMDQ5NTQ4ODUsLTE0MDg4MjI2NDcsLT
ExNzg5NjM0NTUsLTQxNDYwNzA5NiwtNDU2NzI2MTkwLDY4ODE2
ODU2N119
-->