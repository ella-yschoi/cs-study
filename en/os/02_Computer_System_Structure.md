# 02. Computer System Structure

> Reference: [Ewha Womans University Operating System Course - Prof. Ban Hyo-kyung](http://www.kocw.net/home/cview.do?cid=4b9cd4c7178db077)

<br/>

## Types of Operating Systems

### Open/Closed Source by Operating System

- Windows: Closed Source Software
- Linux: Open Source Software
- Android: Open Source Software (includes Linux Kernel at bottom)

### How is Open Source Possible?

- Because software doesn't have material costs except labor costs
- Market where monopoly is possible, so 1st place software sells well and dominates market, but competitors other than 1st place become obsolete
- 2nd place software opens source before becoming obsolete, so users can adapt source code to various environments and gradually increase market share

<br/>

## What is an Operating System?

### What is an Operating System?

- Software layer installed directly above computer hardware that connects users and all other software with hardware

### Meaning of Operating System

- Narrow meaning of OS (Kernel)
  - Core of OS, part that resides in memory
- Broad meaning of OS
  - Concept including not only kernel but also various peripheral system utilities

<br/>

## Purpose of Operating System

- Provide interface to use computer **conveniently**
- **Efficiently** manage computer system resources (both software and hardware)
- **OS handles** so each user doesn't know how hardware operates

### CPU's Role

- CPU is computer's brain, but has no judgment ability, just calculates and executes quickly as written
- Therefore, computer's brain could be considered OS rather than CPU
- CPU **calculates**, memory **remembers**

### OS's Role

- OS can be considered as **ruler's role** of efficiently managing and operating
- Role of appropriately allocating when multiple programs are loaded simultaneously in limited memory space

<br/>

## Classification of Operating Systems

### Concurrent Task Capability

- Single tasking
  - Process **only one task** at a time
  - e.g. In MS-DOS prompt, couldn't execute another command before finishing one command
- Multi tasking
  - Process **two or more** tasks simultaneously
  - Need to solve security/permission issues for multiple users
  - e.g. In UNIX, MS Windows, can execute other commands or programs before one command finishes

### Number of Users

- Single user
  - e.g. MS-DOS, MS Windows
- Multi user: simultaneous access
  - UNIX, NT server
  - Need to solve security/permission issues for multiple users

### Processing Method

- Batch processing system
  - **Collect certain amount** of job requests and process **all at once**
  - Must wait until job is completely finished
  - e.g. Early punch card processing system for I/O
  - e.g. 'I won't show results immediately thinking of user, I'll do it my way!'
- Time sharing system
  - **Divide computer processing power into fixed time units** when performing multiple tasks
  - Has **shorter response time** compared to batch processing system
  - **Interactive** method: thinking of user, shows **immediate results**
  - Even when using computer simultaneously, divides time to make it seem like **using alone**
  - e.g. UNIX, systems used by ordinary users
- Real-time system
  - OS for real-time systems where certain work **must be guaranteed to finish** within fixed time
  - Concept of **Deadline** exists and must be satisfied. Violation causes very fatal results
  - e.g. Nuclear reactor/factory control, missile control, semiconductor equipment, robot control
  - Hard real-time system
    - Violating deadline is quite fatal
  - Soft real-time system
    - Violating deadline is relatively less fatal
    - e.g. When video player frame doesn't render, video cuts off

### Terminology Summary

Below terms have same meaning of performing multiple tasks simultaneously in computer, but emphasize slightly different aspects.

- Multi-tasking
- Multi-process
- Multi-programming
  - Emphasizes memory aspect
  - Meaning that multiple programs are loaded on memory simultaneously
- Time sharing
  - Emphasizes CPU aspect
  - Mainly meaning of dividing and using CPU time

Below term has completely different meaning

- Multi-processor
  - Meaning that multiple CPUs (Processors) are attached to one computer
  - However, not common environment, dealt with in high-performance computing, cloud computing fields

<br/>

## Examples of Operating Systems

### UNIX

- History
  - Originally hardware management was also needed, development was very difficult with assembly language
  - Later with C language emergence, development and modification became easier and more understandable for people
- Features
  - Most code written in C language
  - C language is compatible across various architectures, so has **high portability**
  - OS for servers → Therefore needs to manage multiple programs, multiple users
  - Minimal Kernel structure
  - Open source
  - Easy program development and expansion
  - Various versions (e.g. Linux case, being developed based on this since it's open source)

### DOS (Disk Operating System)

- History
  - Developed by MS for IBM-PC in 1981
- Features
  - Single user OS
  - Limited memory management capability (main memory: 640KB)

### MS Windows

- Features
  - MS's multi-tasking GUI-based OS
  - Plug and Play: Can use immediately without separate user operation or program installation when connecting hardware to computer
  - Enhanced network environment
  - Provides compatibility with DOS applications
  - Rich support software

<br/>

## Operating System Structure

![OS_structure](/Images/OS_structure.png)

<br/>

## Computer System Structure

![computer_system_structure](/Images/computer_system_structure.png)

<br/>

## Mode Bit

### What is Mode Bit?

- **Protection mechanism** to prevent damage to other programs and **operating system** from user program's incorrect execution

### Need for Mode Bit

- Because when CPU goes to user program, **OS cannot control**
- Need **distinguishing entity** to determine whether OS or user program is executing when CPU executes machine language
- When CPU is not in controllable position (when doing other work), mode bit is used for permission control
- That is, Mode bit plays role of **protecting system** by allowing only **safe machine language execution**

### Types of Mode Bit

Mode bit supports two modes of operation through hardware

#### 0 (Monitor mode, Kernel mode, System mode)

- When OS is executing CPU (during OS code execution) **can do anything**
- However, when OS hands over to user program, changes to 1 and hands over
- Privileged instructions (dangerous instructions) can only be executed by OS when 0

#### 1 (User mode)

- User program execution
- Allows user program to execute only **safe machine language** with CPU

### Mode Bit Principle

- Important instructions that can harm security are **privileged instructions that can only be executed in monitor mode(0). That is, dangerous instructions**
- Conversely, when user program has CPU, if it's user mode(1) even though it's privileged instruction, after confirming execution of machine language without permission, CPU automatically goes to OS (→ called Interrupt and Exception)
- When Exception occurs, switches back to monitor mode(0) and CPU automatically goes to OS

- Must execute in monitor mode(0) until handing CPU to user program, and Mode bit is set to 1 during user program execution

### Mode Bit Transition Method

![mode_bit](/Images/mode_bit.png)

#### Exception

- When executing machine language without permission, **Mode bit changes to 0** and CPU usage rights go to OS
- This means Exception that occurs when trying to execute unauthorized work

#### Interrupt Overview

- Check if Interrupt from I/O devices has come in line while reading machine language every moment, before reading next machine language
- If Interrupt has come, CPU automatically goes to OS → Mode bit changes to 0 → OS catches CPU and responds to that Interrupt
- This means CPU Interrupt (from Disk controller, I/O controller, etc.)

<br/>

## Registers

![registers](/Images/registers.png)

### What are Registers?

- Fast and small-sized storage for storing operation input and output, attached to CPU

### Types of Registers

#### PC (Program Counter) Registers

- Holds memory address of machine language to execute next → meaning pointing to address
- CPU executes machine language at memory address PC Registers points to
- When execution ends, executes machine language at next position
- When CPU Interrupt occurs, PC Registers looks at OS memory address, and authority goes to OS (that is, mode bit becomes 0)

<br/>

## Timer

### What is Timer?

- Taking away CPU usage rights from program

### Timer's Role

- Generates Interrupt after certain time passes → **Role of taking away CPU usage rights**
  - OS cannot take away CPU usage rights alone
  - **Generate Interrupt** so control goes to OS after fixed time passes
  - Need **additional hardware** to prevent CPU monopoly → Timer
- Widely used to implement Time sharing, also used for time calculation purposes

### Timer Operation Method

- Timer decreases by 1 every clock tick
- When Timer value becomes 0, Timer Interrupt occurs
- When OS hands over to user program, doesn't just hand over, but sets time in Timer and hands over
- Therefore even if want to use CPU continuously in infinite loop, Timer interrupts CPU so CPU control goes to OS → OS hands over CPU usage rights to other program

<br/>

## Additional Interrupt Explanation

![interrupt_line](/Images/interrupt_line.png)

### What is Interrupt?

- Save Registers and PC Registers at interrupted point, then hand over CPU control to Interrupt handling routine
- That is, Interrupt line attached to CPU is set so CPU control automatically goes to OS before executing next machine language

### Two Meanings of Interrupt

- Interrupt (Hardware Interrupt)
  - Interrupt generated by hardware, more general meaning of Interrupt
  - e.g. Timer, Disk Controller, etc.
- Trap (Software Interrupt)
  - Interrupt generated by software for individual program to hand over CPU to OS
  - e.g. Exception, System Call

### Interrupt Related Terms

- Interrupt Vector
  - Holds address of corresponding Interrupt handling routine
  - Contains location of code to execute for each Interrupt type, which OS code to execute
  - Can be said to be **kind of pointer to address**
- Interrupt handling routine (=Interrupt Service Routine, Interrupt Handler)
  - Kernel function that handles corresponding Interrupt
  - When going to fixed address, what to do is defined in machine language

<br/>

## System Call

![system_call](/Images/system_call.png)

### What is System Call?

- User program calling Kernel function to receive OS service
- If want to do I/O but only OS can do it, interrupt itself → System Call
- When want to hand over CPU to OS, cannot directly hand over PC(Program Counter), so program sets Interrupt line through its machine language → Interrupt occurs (here Software Interrupt)

### System Call Principle

- All instructions requesting I/O from CPU are **bundled as privileged instructions**
  - That is, user program cannot execute directly
  - Because privileged instructions cannot be executed when mode bit is user mode(1)
- Therefore need to request OS to do it instead (System Call)
  - Machine language executes at user program location → hands over CPU usage rights to OS
  - At this time, jump occurs where machine language executes (jump across program's virtual memory)

<br/>

## Device Controller

### What is I/O Device Controller?

- Kind of small CPU that manages corresponding I/O device type
- Has control register, status register for control information
- Has local buffer (kind of data register)

### Device Controller Principle

- I/O happens between actual device and local buffer
- **Device Controller notifies CPU of that fact through Interrupt when I/O ends**
- Code executed in Device Driver is called 'firmware'
- Works because pre-coded program called firmware of I/O device is included

### Device Controller Terminology

- Device Driver
  - OS code for each device processing routine → software
  - Machine language that CPU requests to device controller
  - Code that CPU executes inside computer
- Device Controller
  - Kind of small CPU that controls each device → hardware
  - Not code that Device Controller executes, but code that CPU executes

<br/>

## Cases Where CPU Goes to OS

### (1) True Interrupt

- Cases where Hardware devices interrupt and CPU goes over

### (2) System Call

- Cases where user program Software directly sets Interrupt line and CPU goes over

### (3) Trap

- Cases where CPU goes over by program's own request (cannot be called Interrupt)
- However, broad meaning Interrupt, that is, Trap interpretation

### (4) Exception

- Cases of executing machine language without permission
- Cases where interrupt occurs by itself and CPU goes over

<br/>

## Cases Where CPU is Released

Cases where user program has CPU and hands over or releases to other program or OS

### (1) Involuntary

- Cases where want to use CPU but monopoly rights are taken away
- e.g. Timer Interrupt

### (2) Voluntary

- Cases where no longer want to use CPU
- e.g. During long I/O work (because must wait until I/O ends even if have CPU)

<br/>

## Modern OS is Driven by **Interrupt**

- Meaning that OS does nothing and only works when Interrupt comes in
- OS is also a program, so doesn't mean it can take away CPU with powerful authority, but can use **as long as Interrupt comes in**
- When OS role is needed, **always goes over by Interrupt** to prevent CPU monopoly

<br/>

## Synchronous and Asynchronous I/O

![synchronous_asynchronous](/Images/synchronous_asynchronous.png)

### (1) Synchronous I/O

- Cases where work happening in I/O and work happening in CPU must match well in time
- After CPU requests I/O, control goes to user program **only after I/O work is completed**
- That is, after requesting I/O to Disk, when I/O ends controller Interrupts → **look at results and coordinate with each other and proceed to next step**
- **More common** I/O method than asynchronous

#### Synchronous I/O Implementation Method 1

- Waste CPU until I/O ends
- **Only one I/O** can happen at each moment

#### Synchronous I/O Implementation Method 2

- **Take away CPU from that program** until I/O is completed
- Because CPU is wasted if just waiting
- Line up that program in line waiting for I/O processing
- Hand over CPU to other program while waiting

### (2) Asynchronous I/O

- Cases where work happening in I/O and work happening in CPU don't need to match
- After I/O starts, control immediately goes to user program without waiting for I/O work to end
- That is, when CPU requests to Disk, **does next work regardless of request and result**

> Both synchronous and asynchronous notify I/O completion through Interrupt

<br/>

## DMA (Direct Memory Access)

![dma_controller](/Images/dma_controller.png)

![dma_picture](/Images/dma_picture.png)

### Need for DMA

- If Interrupt occurs too frequently → overhead
- Therefore used to reduce Interrupt frequency

### What is DMA?

- Mechanism that allows Device Controller to communicate directly with Memory without CPU intervention
- Sometimes used to process I/O devices with fast speed at speed close to Memory
- That is, DMA directly copies data blocks between memory and device without CPU intervention, reducing Interrupt frequency and improving CPU efficiency

### DMA Operation Principle

- Since only CPU can access Memory, originally CPU moves every time by Device Controller's Interrupt → sometimes inefficient
- Device Controller can transmit data blocks to Memory through DMA, and when data block transmission is completed, DMA Controller generates Interrupt to CPU
- To summarize, put one more mechanism called DMA that can directly access Memory → Device Controller **directly puts content read from Device into memory** without CPU Interrupt → when work ends, interrupts CPU to notify **at once**

### DMA Usage Advantages

- If done as above, Interrupt occurs less frequently so CPU can operate efficiently

<br/>

## Different I/O Machine Languages

![special_instruction](/Images/special_instruction.png)

### Two Methods of Performing I/O Through Machine Language

#### (1) Method of Having Machine Language Dedicated to I/O

- When CPU executes machine language, there are separate machine languages for requests
- Separate machine language for accessing Memory, separate machine language for performing I/O
- (Left side in above picture) Device Addresses + Memory Addresses

#### (2) Memory Access Machine Language Also Accesses I/O

- Extend memory address not only to actual memory but also to I/O devices, and access I/O through machine language that accesses memory
- (Right side in above picture) Only Memory Addresses
