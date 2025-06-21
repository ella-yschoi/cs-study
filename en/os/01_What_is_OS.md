# 01. Operating System Overview

> Reference: [Ewha Womans University Operating System Course - Prof. Ban Hyo-kyung](http://www.kocw.net/home/cview.do?cid=4b9cd4c7178db077)

<br/>

## What is an Operating System (OS)?

![what_OS.png](/Images/what_OS.png)

- **Software layer that sits directly above computer hardware and connects users and all other software with hardware**
- Unlike other application software, it must directly manage hardware and provide convenient interfaces

<br/>

## Purpose of Operating System

### (1) Provide **convenient environment for using computer systems**

- Provides the illusion that different users and programs are running on separate computers
- Operating system handles the complex parts of direct hardware manipulation

### (2) **Efficiently manage computer system resources**

- Efficient management of CPU, memory, I/O (Input & Output) devices, etc.
- Maximize performance with given resources → **Efficiency**
- Prevent excessive disadvantage to specific users/programs → **Fairness (round robin concept)**
- Allocate CPU to running programs in short time intervals
- Appropriately distribute memory space to running programs

<br/>

## Computer System Structure

![computer_system.png](/Images/computer_system.png)

### I/O

- Input
  - Data entering devices
- Output
  - Results being output
- e.g. Hard disk (auxiliary storage)
  - Device that can perform both Input and Output
  - Can both process (Input) and store (Output)
  - That is, can perform both read and write operations

<br/>

## Operating System Functions

![os_function.png](/Images/os_function.png)

- Computer booting == Operating system loads into memory
  - Core is kernel: Part that resides in memory and is always loaded
- Rest (Programs A~C that OS schedules for CPU to work): Things that occupy memory space
  - e.g. List that appears when entering `htop` command in terminal
- CPU's workspace == Memory
  - CPU reads machine language every clock cycle and performs operations
- I/O devices have small CPUs attached (I/O controllers)
  - CPU requests 'please read file' → Reads requested file into small memory space → Tells CPU it's done → When executing tasks in CPU queue, CPU loads program into memory and works

<br/>

## CPU Scheduling Overview

![cpu_scheduling.png](/Images/cpu_scheduling.png)

- **Which** program should get CPU usage rights for **how long**?
- CPU only reads machine language and does what it's told (cannot be the subject, only does what the supervisor OS tells it to do. e.g. Like a very smart child who only does what they're told)
- OS tells CPU using machine language it possesses (OS delegation)
- When OS manages CPU, it doesn't let it use infinitely, but only for limited time → OS plays role of taking away usage rights when time passes
- However, scheduling proceeds in cooperation with hardware

<br/>

## Memory Management Overview

![memory_managing.png](/Images/memory_managing.png)

- How to split limited memory for use?

<br/>

## Disk Scheduling Overview

![disk_scheduling.png](/Images/disk_scheduling.png)

- Can process requests in order they came first, but may cause inefficiency
- Since OS purpose is 'efficiency', can change order of later requests to operate efficiently
- e.g. Similar to elevator scheduling
  - Principle of picking up 2nd floor person first even if they pressed button later, since going from 1st to 10th floor anyway

<br/>

## Interrupt and Caching Overview

Methods to overcome speed difference between fast CPU and slow I/O devices

![interrupt_caching.png](/Images/interrupt_caching.png)

### Caching

- Putting intermediate stage
- Instead of processing when request comes to read file from disk (same data might need to be read again), **store somewhere in memory** and read from memory copy

### Interrupt

- **Slow I/O devices notify that work is completed**
- CPU doesn't directly do I/O work but asks disk controller to read file → Meanwhile CPU finds something it can do immediately (→ CPU scheduling) That is, CPU is fast so don't let it idle, let it do anything
- If file reading is done? → Tells CPU that program A's request is completely read → Interrupt
- Since it's inefficient to give work to slow devices and check if CPU's work is done, interrupt is sent when assigned work is done and CPU needs to be contacted (very rarely)

### Interrupt Principle Analogy

- Team leader assigns work to team members A, B, C → Team member A finishes work → Leaves interrupt post-it note and leaves → Team leader is interrupted after work is done and checks team member A's work
- That is, CPU temporarily delegated disk-related work to disk controller, so when controller is done → interrupt → CPU does work after reading is completed (showing, etc.)
- Blocking: Team leader holds and checks if team member did work well
- Non-blocking: Team leader and team member do their work separately

<br/>

## Process States

![process_queue.png](/Images/process_queue.png)

- Since there's only one CPU, machine language execution happens inside CPU
- Others can't use CPU and wait, but OS creates 'queue' for those who want to use CPU → Lines them up so they can use CPU for short time
- Need to read file from disk → Process piles up in disk I/O queue → Processed by CPU scheduling → interrupt → Remove from disk queue → Put in CPU queue
- Interactive Application
  - Programs that interact with people (games, web surfing, etc.)
- Scientific Application
  - Programs that use CPU for very long time without I/O work

<br/>

## Additional CPU Scheduling Concepts

![cpu_scheduling.png](/Images/cpu_scheduling.png)

### Role

- One of OS's important roles
- When multiple programs want to use CPU and wait in CPU queue, decides in what order to execute tasks in queue

### Example

- 3 processes line up in queue wanting to use CPU
- All arrival times are same but slight difference makes arrival order p1 → p2 → p3
- User keyboard input → Keyboard controller interrupts CPU with input content → Goes to OS and OS notifies that work has arrived and puts in CPU queue → Scheduled and turn comes → Operation → Output operation result to screen → (repeat this order)

### (1) FCFS (First-Come First-Served)

![cpu_FCFS](/Images/cpu_FCFS.png)

- Feature: Schedule in **order requests entered CPU queue**
- Advantage: Fair since requests are in order they came → **Fair**
- Disadvantage: If first request occupies CPU for long time, average waiting time of later tasks becomes long → **Inefficient**

### (2) SJF (Shortest-Job-First)

![cpu_SJF](/Images/cpu_SJF.png)

- Feature: **Schedule process with shortest CPU usage time first**
- Advantage: Guarantees minimum average waiting time → **Efficient**
- Disadvantage
  - May have to wait indefinitely (Starvation occurs) → **Low fairness**
  - So programs with long usage time may never use CPU

### (3) Round Robin (RR)

![cpu_Round_Robin](/Images/cpu_Round_Robin.png)

- Feature
  - Most commonly used method today
  - Each process has **same size CPU allocation time**
  - That is, **alternate usage in short time** method
- Process
  - When allocation time ends, Interrupt occurs → CPU is taken away → Line up at back of CPU queue
  - Programs wanting short CPU usage get what they want and go to I/O when they get CPU, long usage programs repeat getting and losing → Long processing tasks continue unfinished work when turn comes again → Exit CPU queue when finished (However, OS alone cannot take away CPU, needs hardware help)
  - When n processes are in CPU queue, no process waits more than (n-1)\*allocation time
- Advantage
  - Programs wanting long CPU usage have longer wait time in queue
  - However, programs wanting short usage have shorter wait time
  - That is, wait time is proportional to process's CPU usage time

<br/>

## Additional Memory Management Concepts

![memory_swap](/Images/memory_swap.png)

### Virtual Memory

- Exists because **doesn't go directly to memory, but goes through one more stage**
- Only **immediately needed parts of virtual memory are split and placed** in actual physical memory

### File System Disk

- Stores program executable files
- **Content remains even when power goes out**

### Swap Area

- When physical memory is full, some processes must be evicted, they are moved to Swap area
- Located on persistent disk as memory extension space
- **Survives when power goes out but meaningless** (since that process is no longer running)

### Then, what should be evicted from memory to Swap area?

![memory_LRU_LFU](/Images/memory_LRU_LFU.png)

- Unit of splitting: 'Page'
- Process
  - Don't evict pages likely to be used again in future,
  - Evict pages unlikely to be used in near future for efficient management
  - That is, in situation where future is unknown, don't evict those likely to be used again
- LRU (Least Recently Used) method
  - Delete pages referenced longest time ago first
- LFU (Least Frequently Used) method
  - Delete pages with least reference count first

<br/>

## Additional Disk Scheduling Concepts

![disk_scheduling.png](/Images/disk_scheduling.png)

![disk_seektime](/Images/disk_seektime.png)

### Need for Disk Scheduling

- Most time in access time is disk head movement time → Therefore must minimize head movement time (seek-time)
- Moreover, rotation delay time after head movement time also takes most time
- Actual data transfer time is very short → Reason scheduling is needed

### (1) FCFS (First-Come First-Served)

![disk_FCFS](/Images/disk_FCFS.png)

- Feature: Move head in order requests entered disk queue
- Disadvantage: Head movement becomes long → **Inefficient**

### (2) SSTF (Shortest Seek Time First)

![disk_SSTF](/Images/disk_SSTF.png)

- Feature: Among requests in queue, schedule in order of shortest seek time (closest) from current position
- Advantage: Movement distance becomes much shorter → **Efficient**
- Disadvantage: Same as CPU scheduling disadvantage → **Starvation problem** occurs

### SCAN

![disk_SCAN](/Images/disk_SCAN.png)

- Feature
  - Not concerned with how requests are in queue
  - Process all requests on the way → 'I go my way!'
  - Similar to elevator scheduling principle
- Advantage
  - Head movement becomes faster → **Efficient**

<br/>

## Storage Hierarchy and Additional Caching Concepts

![save_caching](/Images/save_caching.png)

### Storage Hierarchy Features

- CPU executes machine language using values in Registers
- Cache Memory placed in middle to reduce speed difference between Registers and Main Memory
- Today Flash memory exists between Main Memory and Magnetic Disk
- As you go down, slower and cheaper so can use more capacity

### Primary Area Features

- **Volatile**: Doesn't remain when power goes out
- Composed of DRAM (Dynamic Random Access Memory: very fast storage used in computer memory)
- Main memory hierarchy, CPU can directly access and execute
- Located inside computer

### Secondary Area Features

- **Non-volatile**: Remains when power goes out
- Hard disk composition
- CPU cannot directly access, so place in primary and access
- I/O devices

### Reason for Hierarchical Structure

- Has layered architecture to buffer speed differences
  - Final place where data accumulates: bottom
  - If needed from above, bring up and process
  - If need to store, bring down and store

### How to buffer speed differences → **Caching**

- When certain data is needed, bring it up and use
- But if same data is requested again, can read from intermediate path for reuse
- That is, copy to fast layer and just give that copy
- Of course not all data can be copied, but like memory management logic, when evicting pages, must evict those unlikely to be reused → What to keep and what to evict!

<br/>

## Flash Memory

![flash_memory](/Images/flash_memory.png)

### What is Flash Memory?

- Semiconductor device like DRAM
- Flash memory we use can be considered mostly NAND type

### Features

- Hard disk consumes much power since it needs to rotate disk, but flash memory consumes less power
- Strong against physical shock
- Small and light, convenient to carry
- Therefore good for mobile devices (runs on battery)

### Usage Forms

- Started with mobile devices initially
- Later expanded to SSD as hard disk replacement
- Currently expanding to data center usage

### Disadvantages

- Limited write count
- Important data in hard disk can usually be read even after long time
- However, flash memory data may deteriorate over time

<br/>

## Types of Operating Systems

- Server, PC, smart device operating systems
- Open Source Software
  - Linux, Android
