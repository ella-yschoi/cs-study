# 03. Process Management

> Reference: [Ewha Womans University Operating System Course - Prof. Ban Hyo-kyung](http://www.kocw.net/home/cview.do?cid=4b9cd4c7178db077)

<br/>

## Program Execution (Memory load)

![memory_load](/Images/CS03_memory_load.png)

### Principle

- Program is stored as executable file
- When executed, that program loads into memory and becomes process
- At this time, OS Kernel is already loaded in memory
- When program executes, its own independent Address Space → Virtual memory
- If immediately needed parts are loaded into physical memory
- Parts not loaded in memory go into Swap area
- Some of these exist as files in file system

### Address Translation

- Translation needed between Virtual Memory Address (logical address) and Physical Memory Address (physical address)

### Virtual Memory

#### stack

- Local variable data inside functions are located
- Information related to function calls and returns are stacked up

#### data

- Global variable data are located
- Values that exist from program start to end are also located

#### code

- Code that was in executable file loads
- Machine language to be executed by CPU is located → Compiled machine language code

### Kernel Address Space

- Kernel is also a program so has function structure
- Therefore composed of stack, data, code identically

<br/>

## Kernel Address Space Contents

![kernel_address](/Images/CS03_kernel_address.png)

### Structure

- As one program, has function structure
- Each process's Address Space is composed of Code, Data, Stack

### OS Code

> Can be considered that mainly what OS does is inside Kernel Code

- System call, Interrupt handling code
- Contains what to process when Interrupt comes in
- Code for efficient resource management
- Code for providing convenient services

### OS Data

- Data structures for managing currently running hardware and all processes
- This is called PCB (Process Control Block)

### OS Stack

- Each process's kernel stack is stored separately to know who is currently running
- Each process has separate kernel
- That is, stored separately according to which process it was executed from before Kernel was called
- Related to Kernel functions since it's Kernel's stack

<br/>

## Functions Used by User Programs

![function](/Images/CS03_function.png)

### User Defined Functions

> Located within corresponding process address space

- Functions I made directly, functions defined in my program
- Just change program counter value to execute machine language at different location
- Included within my program

### Library Functions

> Located within corresponding process address space

- Functions not defined in my program but borrowed
- Not functions I made but put into program
- Like user defined functions, included within my program
- At this time, principle of just changing program counter value within my program to execute machine language at different location

### Kernel Functions

> Located within kernel address space

- Functions of OS program
- Functions that go into kernel, not into program
- When executing and need to read file from disk, etc., I/O cannot be done directly, so must execute kernel function (system call) instead of user defined/library function
- Since it crosses virtual memory address space to different program area, executes after handing over CPU control to OS

<br/>

## Program Execution

![program_execution](/Images/CS03_program_execution.png)

### User Defined Call & Library Function Call

- user mode (mode bit: 1)
- Code in my address space executes in user mode

### System Call

- kernel mode (mode bit: 0)
- Since CPU control goes to OS, code in OS address space executes in Kernel mode

<br/>

## Process Concept

![프로세스](/Images/CS03_프로세스.png)

### What is a Process?

- Means program in execution
- "_Process is a program in execution"_

### Process Context

#### What is Context

- Represents current state of process at current point in time
- Concept that changes over time

#### Hardware context representing CPU execution state

> How far has CPU executed?

- program counter value → where is currently executing
- Various Register values → what values were in Registers

#### Process Address Space

> What does it have in its memory space?

- code, data, stack

### Process Related Kernel Data Structures

#### PCB (Process Control Block)

- Data structure having information about how much CPU, memory, etc. were used
- Can be said to be data structure for managing process since must look at PCB to know context

#### Kernel Stack

- Each process has different kernel stack

<br/>

## Process States

> Process executes while state changes

![프로세스_state](/Images/CS03_프로세스_state.png)

### Running

- State of holding CPU and executing instructions
- Since there's only one CPU, process executing machine language in CPU is one at each moment → said to be in Running state

### Ready

- State of waiting for CPU (satisfying all other conditions like memory)

### Blocked (wait, sleep)

- State where cannot execute instructions immediately even if given CPU due to long work
- State of waiting because event requested by process itself (e.g. I/O) is not immediately satisfied
- When Interrupted, means event in progress is completed, so change to Ready state
- e.g. When need to read file from Disk

### New

- State where process is being 'created'

### Terminated

- State where execution has ended

### Example

- Program waiting for keyboard input is in blocked state → keyboard input comes in → change to ready state → copy keyboard input content to memory so can get CPU (CPU control goes to OS)

<br/>

## Process State Diagram

![프로세스_state_picture](/Images/CS03_프로세스_state_picture.png)

### new

- State being 'created'
- If before start, not a process

### ready

- State where can execute immediately if only CPU is allocated, that is, fully become process
- Parts needing CPU must be loaded in memory

### running

- Becomes running state when CPU scheduler dispatches (hands over CPU)
- Cases where running state ends
  - Timer interrupt: situation where must line up at back of ready queue due to allocation time expiration
  - I/O or event wait: case of entering blocked state due to long work
  - Exit: case of termination

### waiting (blocked)

- When long work ends (usually interrupt occurs), change that process's state to ready to prepare for CPU to execute that process again

### terminated

- State being 'terminated'
- If already terminated, not a process

<br/>

## PCB

![pcb](/Images/CS03_pcb.png)

### Definition

- Data structure that OS maintains to manage each process
- Information maintained per process

### Components

- Information used by OS for management
  - Process State, Process ID
  - Scheduling Information, Priority (CPU allocated to highest priority process)
- Hardware values related to CPU execution
  - Values saved in preparation for Context Switching
  - Program Counter, Registers
- Memory related
  - Location information of code, data, stack
- File related
  - Open file descriptors...

<br/>

## Context Switching

![context_switching](/Images/CS03_context_switching.png)

### Definition

- Process of handing over CPU from one process to another process

### Why Needed

- Not just taking away CPU, but must save current process context so can execute right at next point
- That is, if want CPU to go from process A to process B, save information like where current machine language is executing in process A's PCB before taking away CPU, then hand over CPU

### Operation Process

> When CPU goes to another process, OS performs below operations

- Save state of process giving up CPU in that process's PCB
- Read state of process newly getting CPU from PCB

<br/>

## Distinguishing Context Switch

![context_switch_difference](/Images/CS03_context_switch_difference.png)

### What is context switch

- User process A ↔ User process B

### What is not context switch

- User process A → OS → (again) User process A

### Relationship between context switch and overhead

- Going from user mode to kernel mode has relatively less overhead than context switch
- Must save part of context like CPU execution information in PCB without context switch
- But when doing context switch, that burden is much greater (overhead is very large when flushing cache memory)

<br/>

## Queue for Process Scheduling

> Processes execute while moving between each queue

### job queue

- Set of all processes currently in system

### ready queue

- Set of those among them that need to get CPU immediately
- Set of processes currently in memory waiting to get CPU and execute

### device queue

- Long work
- Set of processes waiting for I/O device processing

<br/>

## Actual Data Structure Form

![data_structure](/Images/CS03_data_structure.png)

<br/>

## Process Scheduling Queue Appearance

![프로세스_scheduling_queue](/Images/CS03_프로세스_scheduling_queue.png)

- When process first executes, ready queue
- When turn comes, get CPU and use, then terminate sometime
- When requesting long waiting work while using → go to I/O queue and line up
- When finished, get right to get CPU again in ready queue
- Want to keep using CPU but when timer interrupt ends
- Go back to ready queue

<br/>

## Scheduler

> Part of code inside OS

### Long-Term Scheduler (or Job scheduler)

> Role of doing memory scheduling within OS

#### Role

- Decide which ones among starting processes to send to ready queue
- Problem of 'giving' memory (and various resources) to process. That is, responsible for making process load into memory when executed
- Control Degree of Multi-programming (number of programs loaded in memory)

#### Features

- Usually no long-term scheduler in general OS Time Sharing System (goes directly to ready state)
- Instead of long-term scheduler, how to manage memory? → Manage with medium-term scheduler

### Medium-Term Scheduler (or Swapper)

> Time sharing system doesn't have long-term scheduler but has medium-term scheduler

#### Role

- When all started programs come into memory → compete so system performance drops, so evict process entirely from memory to disk to make free space
- Problem of 'taking away' memory from process (give all first, then evict from memory when memory is insufficient and overall system performance drops)
- Control Degree of Multi-programming

### Short-Term Scheduler (or CPU scheduler)

> Role of doing CPU scheduling

#### Role

- Decide which process to run next
- Problem of 'giving' CPU to process

#### Features

- Must be fast enough since called very frequently (millisecond units)

<br/>

## Process States

### Suspended (Stopped)

#### Features

- State that occurs when medium-term scheduler is added
- State evicted from memory by medium-term scheduler
- Process is entirely swapped out to disk
- Also state where process execution is stopped for other external reasons

#### e.g. External reasons

- When user temporarily stops program (by break key, Ctrl+Z in Linux environment)
- When system temporarily suspends process for various reasons (when too many processes are loaded in memory and competition is severe) medium-term scheduler evicts process entirely from memory to disk to make free space

### Difference between Blocked and Suspended

#### Common points

- State where cannot get CPU

#### Main differences

- Blocked state
  - Process still remains in memory
  - State where can get CPU while waiting for event
- Suspended state
  - Process is temporarily removed from memory
  - State where performs no work until activated externally

#### Blocked

- State where cannot get CPU while process waits for I/O work or other events (e.g. external resource request)
- State where process waits (by itself) until that event occurs
- When event occurs, process transitions to ready state and can get CPU again

#### Suspended

- State where process is temporarily stopped or halted
- Halted state where cannot do work at all
- Halted state generally requires external reactivation or resumption of process
- Mainly used to remove process state from memory and reload to memory when needed later

<br/>

## Process State Diagram

> State diagram including Suspended

![프로세스_status_active_inactive](/Images/CS03_프로세스_status_active_inactive.png)

### Active / Inactive

#### Active

- Running
- Ready
- Blocked

#### Inactive

- Suspended Blocked
- Suspended Ready

### Swap Out / Swap In

> Used for memory management in OS, plays important role in managing processes in memory shortage situations

#### Swap out

- When process or program is currently loaded in memory (RAM)
- Means work of making that process or program "entirely evicted" from memory for system to secure memory space
- At this time, that process's data and code are stored in disk's Swap File or Swap Area

#### Swap in

- When swapped out process or program needs to execute again
- Means work of system "loading" that process's data and code stored in swap file or swap area back to memory
- This makes that process executable again in memory

### Suspended(Blocked) / Suspended(Ready)

#### Common points

- State where memory is entirely taken away
- Cannot do CPU work since memory is entirely taken away, but some I/O work can proceed
- Must allocate memory again externally to become active state

#### Suspended(Blocked)

- Process that was doing I/O work before entering Suspended state
- I/O work is temporarily suspended and enters "Suspended Blocked" state
- When I/O work is completed, can return to "Suspended Ready" state

#### Suspended(Ready)

- Completed state, meaning not doing I/O work

### Running (User mode / Kernel mode)

![프로세스_status_running](/Images/CS03_프로세스_status_running.png)

#### Important concept

- State mentioned here exists for OS to manage user programs, not meaning OS's own state
- Running state represents process currently using CPU or OS Kernel mode code
- When process is temporarily suspended due to Interrupt or System Call, CPU performs other work

#### When in Running state in User mode

- State where process's own code is executing
- Uses CPU to perform that process's instructions

#### When in Running state in Kernel mode

- State where OS kernel code is executing
- Mainly occurs when performing OS core functions like System Call or Interrupt handling

#### When process calls System Call in User mode

- That System Call executes in kernel mode
- That program is not considered to have CPU taken away, so considered to be using CPU
- But System Call executing in Kernel mode processes process's request and returns to User mode

#### When Interrupt comes in

- Interrupt occurs when other events (e.g. I/O completion, hardware events) occur while process is executing
- In this case, currently executing process transitions to Blocked state, then OS executes in Kernel mode to handle that Interrupt
- This way, currently executing process is temporarily suspended, but CPU is still in use and considered running state in Kernel mode for other reasons

<br/>

## Thread

> A thread (or lightweight process) is a basic unit of CPU utilization : CPU execution unit

### Explanation

- Thread is having only parts needed for CPU execution separately from process
- Process is shared, but thread has each according to work

### Parts that Thread shares with colleague threads (=task)

> Traditional concept's heavyweight process can be considered as task having one thread

- code section
- data section
- OS resources

### Thread Composition

<p align="center" width="100%"><img width="1010" alt="thread-composition" src="https://github.com/ella-yschoi/TIL/assets/123397411/438efd77-c002-4392-840a-7b4439fd468c">

- program counter
- register set
- stack space
  - In address space, thread only has stack related to function calls
  - Rest is shared with other threads within process

### Thread Efficiency

<p align="center" width="100%"><img width="1010" alt="thread-efficiency" src="https://github.com/ella-yschoi/TIL/assets/123397411/f68d530d-c37f-4d00-91b7-6beb3ff499a9">

- It's efficient to have only CPU execution related parts separately as threads from process, and manage rest as single process
- That is, using threads means process communication and data sharing can be done more efficiently. Multiple threads work within same process and share process's memory and resources, so communication cost is reduced
- Context switch has large overhead, but going from thread to thread is efficient
- Opening multiple web browsers creates multiple processes so inefficient, so creating threads is efficient
- However, Chrome browser has security issues so opens multiple processes (Multi-process Architecture) and implementation method may differ by browser
  - [Chrome for Developers - Inside look at modern web browser (part 1)](https://developer.chrome.com/blog/inside-browser-part1/)
  - [[위 블로그 한국어 번역] 크롬 브라우저는 어떻게 작동 할까? - 01](https://onlydev.tistory.com/80)
  - [The Chromium Projects - Multi-프로세스 Architecture](https://www.chromium.org/developers/design-documents/multi-프로세스-architecture/)

### Thread Advantages

<p align="center" width="100%"><img width="1010" alt="thred-pros" src="https://github.com/ella-yschoi/TIL/assets/123397411/67207b0b-1b79-48cc-81ad-421bd23cf6c1">

#### Responsiveness (Fast response)

- While one thread reads through network, another thread shows on screen, so can provide fast response to user

#### Resource Sharing

- Since threads within same process can share most things except CPU execution information, efficient resource sharing is possible
- Since threads execute within same process, can share process's code, data, resources
- This makes resource sharing simple and reduces overhead like copying data
- That is, multiple threads can share process's binary code, data, resources

#### Economy

- Difference between having multiple processes or multiple threads within process for same work
- creating & CPU switching thread (rather than a process)
- In case of solaris (multi-thread model developed by Oracle), above two overheads are 30 times and 5 times respectively

#### Utilization of Multiprocessor Architectures (Parallelism)

- Can pursue parallelism in Multiprocessor (situation where there are multiple CPUs)
- For example, if doing large matrix multiplication, must do sequentially one column at a time if there's one CPU. If there are multiple CPUs, divide matrix multiplication into multiple threads and assign to different CPUs to perform operations in parallel → efficient

### Advantages of Multi-thread Structure

- In task structure composed of multiple threads, even when one server thread is in blocked(waiting) state, other threads within same task can execute(running) for fast processing
- Multiple threads performing same work can cooperate to achieve high throughput and performance improvement. Therefore when using threads, can increase parallelism
- For example, if need to show what can be shown immediately in web browser through HTML document, but takes long time due to long work like getting image URL, while one thread reads, another thread can quickly show text that can be expressed immediately

### Thread Implementation Methods

<p align="center" width="100%"><img width="1010" alt="thread-supported" src="https://github.com/ella-yschoi/TIL/assets/123397411/ec3b56b7-9c1f-4d3b-92ed-e95878431c18">

#### Kernel Threads

> supported by kernel

- Implementation where OS already knows thread's existence
- Use threads directly supported by OS kernel
- OS can do CPU switching between different threads (like CPU scheduling between processes)

#### User Threads

> supported by library

- Manage threads at user program level
- Implementation where OS doesn't know thread's existence
  - Looks like process without threads to OS
- OS cannot do work of handing over CPU between threads
  - Means managing at user program level like requesting asynchronous I/O to OS within process and immediately receiving back and handing over CPU to other thread

<br/>

## Process Management

> How processes are created and terminated

### Process Creation

#### Creation Principle

- When parent process creates child process, creates by copying (process creates another process)
- Creates process with same age as parent
- That is, not creating directly, but creating through system call request to OS (fork system call)
- Forms process tree (hierarchical structure)

#### Process needs resources

- Receive from OS
- Share with parent (actually parent and child processes are separate processes so compete to allocate resources)

#### Resource Sharing

- Model where parent and child share all resources
- Model sharing some
- Model not sharing at all

#### Execution

- Model where parent and child coexist and execute
- Model where parent waits until child terminates (blocked state)

#### Address Space

- Child copies parent's space (binary and OS data)
- Child loads new program in that space
- That is, copy parent's as is so code is copied, but data and stack are also copied → take global variables etc. as is, and copying stack means child executes from current execution position

#### Running multiple programs?

- Means copying first (fork system call), then overwriting different program there (exec system call) to run new program

#### Unix Example

- fork() system call creates new process
  - Copy parent as is (OS data except PID + binary)
  - Allocate address space
- Load new program in memory through exec() system call following fork

### Process Termination

- After process performs last instruction, notifies OS (exit)
  - exit system call → OS terminates process while returning all resources and terminates
  - Child sends output data to parent (via wait)
  - Process's various resources are returned to OS
- When parent process forcibly terminates child's execution (abort)
  - When child exceeds allocated resource limit
  - When task allocated to child is no longer needed
  - When parent terminates (exit)
    - When parent process terminates, OS kills child before parent dies
    - Gradual termination (kill descendants from bottom of hierarchy sequentially)
- Usually child terminates first → parent does aftermath (exit)
  - Child sends output data to parent
  - Process's various resources are returned to OS
- When closing browser window, all running things terminate

### fork() system call

- Process is created by fork() system call
  - Function requesting OS to create child
  - Creates new address space by copying caller

### Distinguishing Parent Process and Child Process

<p align="center" width="100%"><img width="1010" alt="프로세스-end" src="https://github.com/ella-yschoi/TIL/assets/123397411/ecc4b973-846b-4411-b202-723daf57324f">

- Parent: receives PID value greater than 0 as return value
- Child: receives PID value of 0 as return value

### exec() system call

<p align="center" width="100%"><img width="1010" alt="exec-systemcall-1" src="https://github.com/ella-yschoi/TIL/assets/123397411/1dabcb46-8ca2-4b55-a017-0af035443df1">

- After outputting hello, overwrites with new program

<p align="center" width="100%"><img width="1010" alt="exec-systemcall-2" src="https://github.com/ella-yschoi/TIL/assets/123397411/de7761d1-06e4-4bab-a7bb-0ca99b3127c0">

- When want to create child process and run different program (when overwriting new program to child)
  - Parent doesn't just overwrite but executes fork() to copy
  - Parent continues executing originally running program
  - Execute exec() for child to run different new program instead of existing program

### wait() system call

<p align="center" width="100%"><img width="1010" alt="wait-systemcall" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/20e424e8-8b68-49ab-9836-b1042bf670d3">

- When process A calls wait() system call
  - Kernel puts process A to sleep until child process terminates → blocked state
  - When child process terminates, parent gets CPU and kernel wakes up process A → ready state
- In Linux, when entering command, basically one line that can receive input is one process (Shell)
  - Can input another command only after one command terminates
  - When executing one command, executes as child process form
- Summary
  - Parent process waits in blocked state until child process terminates
  - When child terminates, wakes up again and can work

### exit() system call

#### Voluntary termination

- After performing last statement, terminates through exit() system call
- Compiler puts it at position where main function returns even if not explicitly written in program

#### Involuntary termination

- Parent process forcibly terminates child process
  - Child process requests resources exceeding limit
  - Task allocated to child is no longer needed
- When entering kill, break through keyboard
- When parent terminates
  - Children terminate first before parent process terminates

<br/>

## Process Cooperation

### Independent Process

- Since processes execute with their own address spaces, in principle one process cannot affect another process's execution
- Process always operates independently

### Cooperating Process

- One process can affect another process's execution through process cooperation mechanism
- Sometimes process cooperation is needed
- Cannot see other process's memory and cannot give/receive anything with other process, so cooperate by giving/receiving information through IPC as below

<br/>

## Process Cooperation Mechanism (IPC: Inter-Process Communication)

<p align="center" width="100%"><img width="1010" alt="IPC" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/f9fa2ddb-8fcd-4c6d-909a-d8430741ff7f">

### (1) Message Passing

Use method of passing messages through kernel, passing messages to OS kernel so other process receives transmission from kernel

#### Message System

System that communicates without using any shared variables between processes

#### Direct Communication

Explicitly shows name of process to communicate with, explicitly shows who to send message to, explicit message passing method agreed between both parties

<p align="center" width="100%"><img width="1010" alt="Direct-Communication" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/7c8374c7-dc57-46ef-863a-e5002d71ccc2">

#### Indirect Communication

Indirectly pass messages through mailbox (or port), don't specify target when passing, let one of cooperating processes take it out

<p align="center" width="100%"><img width="1010" alt="Indirect-Communication" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/d1c4201a-1410-4d5f-963f-e3079cfacd80">

### (2) Shared Memory (Method of sharing address space)

- There's shared memory mechanism that makes even different processes share part of address space
- Request OS through system call to share part of memory (sharing memory means cooperation is possible)
- However, should only share when can trust each other
- Note) Thread
  - Thread is actually one process so difficult to see as process cooperation, but threads constituting same process can cooperate since they share address space
