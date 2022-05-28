---
title:  Knowledge Framework of OS PART I CH1&CH2
comments: true
toc: true
categories:
  - OS
tags:
  - 专业课
  - OS
  - note 
abbrlink: 28913
date: 2022-2-28 22:52:11
cover:  https://user-images.githubusercontent.com/74918703/155833614-94542aa5-0d30-4691-95d6-9d35a95eaa6a.png
mathjax: true

---

# Knowledge Framework of OS :PART I CH1&CH2

textbook: Operating System Concepts 10th Edition [check](https://www.os-book.com/OS10/index.html)

course home : http://staff.ustc.edu.cn/~ykli/os2022

## ch1 Overview of an Operating System

### system organization

computer-system organization

-  One or more CPUs, device controllers connect through common bus providing access to shared memory 
-  Concurrent execution of CPUs and devices competing for memory cycles
- **I/O devices and the CPU** can execute **concurrently**
- Each device controller is in charge of a particular device type
-  Each device controller has a local buffer 
- CPU moves data from/to main memory to/from local buffers 
-  Device controller informs CPU that it has finished its operation by causing an **interrupt**

computer start-up

1. **bootstrap program** (启动程序)is loaded at power-up or reboot (firmware)
2. System processes or system daemons
3. After fully booted, waits for events to occur – Signaled by interrupt

**interrupt handling**  :can be triggered by hardware and software:

1. Hardware sends signal to CPU 
2. Software executes a special operation:**system call**
3. procedure :CPU stop->executes the service routine->CPU resumes
4. operating system is interrupt driven
5. interrupt architecture must save the address of the interrupted instruction
6. interrupt timeline:

<img src="https://user-images.githubusercontent.com/74918703/155979009-4974c76f-6119-408f-a6e8-86ceb7b4b9d5.png" alt="image" style="zoom:80%;" />

Interrupt transfers control to the interrupt service routine . A table of pointers to interrupt routines, the **interrupt vector**,can be used to provide necessary speed .The table of pointers is stored in low memory.

### Storage Structure

<img src="https://user-images.githubusercontent.com/74918703/155979598-8e6b2eb2-38a1-4280-b98c-bfbb41561456.png" alt="image" style="zoom: 80%;" />

- Main memory :random access,typically small size and volatile(易失性)
- Instruction-execution cycle 1.Fetch an instruction from memory and store in register – Decode instruction (fetch operands if necessary) 2. Store result back to memory
- Secondary storage – extension of main memory that provides large nonvolatile storage capacity (like heard disks ,solid-state disks--faster and nonvolatile)
- caching--small ,important principle,in hardware ,opearting system,software.
- I/O structure:interrupt driven
- Direct Memory Access Structure:Device controller transfers blocks of data from buffer storage directly to main memory without CPU intervention,one interrupt per block

### System Architecture(处理器)

CPU--most use a single general-purpose processor

**Multiprocessors system**:parallel systems,multicore systems,

advances:Increased throughput;Economy of scale;Increased reliability

two types:

**SMP**:symmetric multiprocessing,all processors are peers

<img src="https://user-images.githubusercontent.com/74918703/155982308-fe37e15e-cef6-4597-9f3d-8591f500f1b8.png" alt="image" style="zoom:67%;" />

**AM**:asymmetric multiprocessing,boss-worker relationship



Multicore:: include multiple cores on a single chip .More efficient and less power

<img src="https://user-images.githubusercontent.com/74918703/155982347-15bd8cd7-500d-483f-95c5-3d0ef6cfc6d1.png" alt="image" style="zoom:67%;" />

distinguish:multicore &SMP,multicore is more efficient but costs more.

**Clustered Systems**:multiple systems working together

​		-**SAN**:storage-area network

​		-some are **HPC**:high-performance computing

​		-**DLM**:distributed lock manager,to avoid conflicting



So...

Where is the OS?

> Four components of a computer system 
>
> – Hardware – provides basic computing resources (CPU, memory, I/O devices) 
>
> – Users: People, machines, other computers
>
>  – App. programs – define the ways in which the sys. resources are used to solve the computing problems
>
> ​			 • Word processors, compilers, web browsers, database systems, video games, etc.
>
>  – Operating system 
>
> ​			• Controls and coordinates use of hardware among various applications and users

What is the OS?

> •It stands between the hardware and the user. 
>
> – A program that acts as an intermediary between a user of a computer and the computer hardware 
>
> • Operating system goals: 
>
> – Execute user programs & make solving user problems easier 
>
> – Make the computer system convenient to use
>
>  – Use the computer hardware in an efficient manner 
>
> – Design tradeoff between convenient and efficiency
>
> • How good is this design? 
>
> – The user does not have to program the hardware directly
>
> • Processes as the starting point! 
>
> – Whatever programs you run, you create processes. 
>
> ​		• i.e., you need processes to open files, utilize system memory, listen to music, etc.
>
>  – So, process lifecycle, process management, and other related issues are essential topics of this course



What Operating Systems Do?

- system view
  - control program
  - resource alocator
- User view
  - wanr convenience in use and performance
  - usability ,battery life, resource utilization,tradeoff...

well..OS has no universalyy accepted defination

### Operating Systems Operations

#### OS Operations

control programs/resource allocator

1. **Mutliprogramming**:needed for efficiency,job run via job scheduling
   1. Time sharing,分时操作系统,logical extension in which CPU switches jobs so frequently that users can interact with each job while it is running, creating interactive computing
   2. allow many user to share the computer
   3.  issues:– If several jobs ready to run at the same time:**CPU scheduling** . If processes don’t fit in memory, **swapping** moves them in and out to run . **Virtual memory** allows execution of processes not completely in memory
2. **Interrupt Driven Mechanism**
   1. (in  Multiprogramming)software and hardware
   2. Hardware interrupt by one of the devices
   3. software :(exception or trap),needed to request for opaerating system service
3. **Dual-mode Operation**多模式操作
   1. user mode or kernel mode,and transistion between them



#### system call

system call is similar to a function call ,but it is inside the OS,named OS kernel

- System calls are the **programming interface** between processes and the OS kernel 

  - System calls provide the means for a user program to ask the operating system to perform tasks 

-  A system call usually takes the form of a trap to a specific location in the interrupt vector, **treated by the hardware as a software interrupt**

-  The system call service routine is a part of the OS

- usually primitive,important,fundamental(like time()system call)

- Roughly speaking, we can categorize system calls as follows:

  | Process  | FileSystem | Memory |
  | -------- | ---------- | ------ |
  | Security | Device     |        |

  

- distinguish between system call and library function call(library file:in windows :DLL dynameically linked library ,in Linux SO,shared objects)

OS standards:

<img src="https://user-images.githubusercontent.com/74918703/155991891-c2f45cd5-3191-424d-bd2a-f45d71deec7d.png" alt="image" style="zoom:67%;" />

### Process

#### Process vs Program

A process is an execution instance of a program,a process is not bounded to execute just one program.A process is active and has its own local states

command about processes like ps--report a vast amount of information about every process in system(try ps -ef)

shell---a process launching pad



<img src="https://user-images.githubusercontent.com/74918703/155994339-75d5f4d4-7244-40c0-93a4-a7238a658cee.png" alt="image" style="zoom:67%;" />

System has many processes, some user, some operating system running concurrently

#### Memory

<img src="https://user-images.githubusercontent.com/74918703/155995958-b6202a2b-d093-4966-b04f-ec3f022fe50e.png" style="zoom:67%;" />

Java don't have the above layout,and C is too low

#### Storage Management

**File System,FS**

like FAT16, FAT32, NTFS, Ext3, Ext4, BtrFS

a FS must record :directories,files,allocated,space,free space

Two face of a file system:1.storage design of the file system.(how it stored)2.the opreations of the file systems

operations:creating can be replaced by opening;copying can be replaced by read and write;moving can be replaced by rename(in one disks),so the necessary operations are open,read,write,close,rename,delete,..

A FS is independent of an OS,an OS can use many FS

#### Kernal Data Structures

Lists,Trees,Hash Map and Bitmaps

### MISC

**Protection and Security**

this course will discuss the security of File System

**Computing Environments **

- trandition to mobile:IOS ,Android

- Distributed computing ,like networkTCP/IP

- Client-Server(C-S computing)

- Peer-to-Peer computing--去中心化

-  Virtualization

- Cloud Computing (较成熟)

- Real-Time Embedded Systems

  

**Open-sourced OS**

Open-Source Operating Systems---GNU LINUX,BSD UNIX(including mac OS)



### summary

-  OS Overview 
  -  OS Concept 
  -  Multiprogramming & Multitasking 
  -  Dual Mode & System Call 
-  OS Components 
  -  Process Management 
  -  Memory Management 
  -  Storage Management 
-  Computer System Organization & Architecture 
  -  Interrupt

## ch2 Operating System Structures

### Operating System Services

<img src="https://user-images.githubusercontent.com/74918703/156001574-10982645-91ca-4540-9d1f-de15ab2b1a22.png" alt="image" style="zoom:67%;" />

common classes :Convenience of the user and Efficiency of the system

**Operating systems provide an environment for execution of programs and services to programs and users**

#### User Operating System Interface

execution:1.Load a program into memory 2.run the program3.end execution(normally or abnormally)

for Helping Users

- I/O operations
- File-system manipulation
- communications
- implementations:(**shared memory and Message passing**)
- error detection,error types error handling
for Ensuring Efficiency

for ensuring efficiency

- resource allocation
- accounting--to keep track of
- usage (usage statistics)
- the protection and security 

for Helping Users

- user interface(UI)
  - form: CLI(command line),Batch,GUI(Graphics User Interface)
  - CLI:shells,two ways of implementing commands:
  - 1.The command interpreter itself contains the code • Jump to a section of its code & make appropriate system call • Number of commands determines the size of CLI 
  - 2.Implements commands through system program (UNIX) • CLI does not understand the command • Use the command to identify a file to be loaded into memory and executed • Exp: rm file.txt (search for file rm, load into memory and exe w/ file.txt) • Add new commands easily

touchsreen interface...virtual keyboard?voice command

#### System Calls
**Programming interface** to the service provided by the OS

Each OS ha its own Program Call

System Call && function

Why us API?

for System Call,a simple program like copy may make heavy use of OS,and not user friendly

Mostly accessed by programs via a high-level API rather than direct system call use,and is easy of use(Simple programs may make heavy use of the OS)Program portability(Compile and run on any system that supports the same API )

remember the logic relationship between API and Program calls

API:application programming interface

common APIs:Win32 API for Windows,POSIX API,JAVA API(JAM,java virtual machine) 

How to use API?

1.via a library of code provided by OS
2.Libc:UNIX/Linux with a C language

API-System Call-OS relationship

**Type of System Call**
- Process COntrol
  - example: MS_DOS 
- File Manipulation
- Device Manipulation
- Information Maintenance
- Communications
- Protection



### Operating System Structure

掌握优势和问题，不需要记住例子

- simple structure :MS_DOS
- Monolothic Structure :UNIX

limited structing beyond simple but not fully layered

- Layered Approach

not efficient

- Microkernel System Structure
- Modules
- Hybrid System:combine different systems structure

Examples:Linux,Windows,Mac OS X structure,IOS,Android...



### Operating System Design and Implementation

... is not solvable


1. first problem :design goals and specifications
2. improtant principle to separate(**Mechanism** for how to do  and **policy** for what will be done）
 Examples (**Timer Mechanism** for CPU protection and **Priority Mechanism** in job scheduling)

 benefits:maximum flexbility

OS implementation
- much variation,earily in ass,now in C/C++
- Acutually use a mix of language(body in C,lower levels in ass,system programs in C/C++,scripting language)
- pros and cons 

### MISC:Debugging,Generation &System Boot

**debugging** like GDB
- failure analysis:log files,core dump,crash dump
- performance tuning: trace list of the system behavior & interactive tools(top displays the resources of usage processes)
- Kernighan's LAW:"Debug is twice as hard as writing the code in the first place.Therefore,if you write the code as cleverly as possible,you are ,by defination,not smart to debug it."

**Operation system Generation**---OS is designed to run on any of a class of machines

the system must be configured or generate for each specific computer site


  **SYSGEN** program:it obtains the information concerning the specfic configuration of the hardware system

**system boot**

- system booting on most computer systems
- common bootstrap loader allows selection of kernel from multipule disks,versions,kernel options

Summary of part I :ch1&ch2

-  operating system overview ----**multiprogramming & multitasking**

- OS operations:**system call & dual mode**
- OS components
- computing environment
- OS structure
- process managements
- memory managements
- storage managements
