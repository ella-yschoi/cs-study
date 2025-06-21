# CPU Scheduling

## 1. CPU and I/O Bursts in Program Execution

<p align="center" width="100%"><img width="1010" alt="image" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/05bdab1a-786b-43da-b85d-d39ccee6916f">

<br/>

## 2. CPU-burst Time Distribution

> Stage of executing machine language with CPU

<p align="center" width="100%"><img width="1010" alt="image" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/87bb3ca5-ade5-4bf2-816a-cf4a55822b02">

### Need for CPU Scheduling

- CPU scheduling is needed because various types of jobs (=processes) are mixed
  - Need to provide appropriate response to interactive jobs
  - Use system resources like CPU and I/O devices efficiently and evenly
- Allow I/O bound jobs to get CPU quickly
  - Because must give CPU first to use quickly and exit
  - Need to provide fast responsiveness because interact with people
  - I/O work happens immediately after using, so if cannot give CPU, not only cannot get CPU but also cannot do I/O

<br/>

## 3. Process Characteristic Classification

### (1) CPU-bound process

- Work that uses CPU for long time and I/O for short time
- Cases with much interaction with people (calculation-focused jobs, etc.)
- few & very long CPU bursts

### (2) I/O-bound process

- Work that uses CPU for short time and I/O for long time
- Cases where more time is needed for I/O than time holding CPU and calculating (scientific calculations, etc.)
- many & short CPU bursts

<br/>

## 4. CPU Scheduler & Dispatcher

> Software, code inside OS that does CPU scheduling

### (1) CPU Scheduler

- Role of choosing which process to give CPU to this time among processes in Ready state

### (2) Dispatcher

- Hands over CPU control to process selected by CPU scheduler
- That is, role of actually handing over CPU to decided process
- Part of OS and this process is called context switch

### (3) Cases where CPU Scheduling is Needed

When process has state changes like below

1. Running → Blocked (e.g. system call requesting I/O)
2. Running → Ready (e.g. timer interrupt due to allocation time expiration)
3. Blocked → Ready (e.g. interrupt after I/O completion)
4. Terminate

- Scheduling in 1, 4 is nonpreemptive (= don't forcibly take away, voluntary return)
- Other scheduling is preemptive (= forcibly take away because others must use CPU alternately according to policy)

<br/>

## 5. Scheduling Criteria

- Performance Index (= Performance Measure)
- Easy to understand if compared to Chinese restaurant chef

### (1) CPU Utilization

- keep the CPU as busy as possible
- Don't let CPU idle, higher utilization is better

### (2) Throughput

- number of processes that complete their execution per time unit
- Higher throughput is better
- e.g. Better for Chinese restaurant chef to process many dishes

### (3) Turnaround Time

- amount of time to execute a particular process
- Total time from doing CPU burst to going out to do I/O burst
- Time waited for CPU in Ready Queue + time actually used CPU
- e.g. Time eating + waiting time all combined

### (4) Waiting Time

- amount of time a process has been waiting in the ready queue
- Not time waiting first time, but sum of total time waited after coming to use CPU
- e.g. Time waited at Chinese restaurant.. sum of time each dish comes out: pickled radish → jjajangmyeon → tangsuyuk → dessert

### (5) Response Time

- amount of time it takes from when a request was submitted until the first response is produced, not output (for time-sharing environment)
- Time taken from entering to use CPU until first getting CPU
- Note that it's not time from when process first started to first getting CPU. That is, among multiple bursts entering to use CPU, time taken from entering CPU burst to first getting CPU
- e.g. Time taken to first receive pickled radish at Chinese restaurant

<br/>

## 6. Scheduling Algorithms

### (1) FCFS (First-Come First-Served)

> Much better than previous case

#### Example

<p align="center" width="100%"><img width="1010" alt="fcfs-example-1" src="https://github.com/ella-yschoi/TIL/assets/123397411/cd52e0fb-f323-45cf-8a78-635b4db71df9">

<p align="center" width="100%"><img width="1010" alt="fcfs-example-2" src="https://github.com/ella-yschoi/TIL/assets/123397411/b4fe68b4-6460-4dd5-84fa-b491070a41ff">

#### Features

- Both cases processed first-come-first-served so seem no difference, but when averaging, reduced significantly
- Why? Because if process with long CPU usage time comes first, waiting time of all processes becomes long → Convoy effect

#### Convoy effect

- short process behind long process
- Process that takes long time arrives first and uses CPU for long time, so short process takes long time

### (2) SJF (Shortest-Job-First)

#### Example

<p align="center" width="100%"><img width="1010" alt="sjf-example-1" src="https://github.com/ella-yschoi/TIL/assets/123397411/6a7dcca0-f6b0-4a46-9aec-6a55ffe366d2">

<p align="center" width="100%"><img width="1010" alt="sjf-example-2" src="https://github.com/ella-yschoi/TIL/assets/123397411/b37088c8-2026-4c8f-bf20-aad0f96d5a81">

#### Features

- Use each process's next CPU burst time for scheduling
- Schedule process with shortest CPU burst time first
- From waiting time perspective, most optimal method. Cannot make average waiting time shorter than this

#### SJF is optimal

- Short average waiting time and guarantees minimum average waiting time
- Preemptive SJF == also called SRTF (Shortest-Remaining-Time-First)
- Optimal method from waiting time perspective

#### Allocate CPU to process with High Priority

- SJF is kind of Priority Scheduling
- Priority = predicted next CPU burst time

#### Two schemes

- Nonpreemptive

  - Once get CPU, not preempted until this CPU burst **completes**
  - Even if shorter process appears suddenly, if already gave CPU, don't take away until that process uses it all

- Preemptive
  - **CPU is taken away when new process with shorter CPU burst time than remaining burst time of currently executing process arrives**
  - More optimized method than Nonpreemptive

### (3) SJF Disadvantages

#### Starvation occurs

- SJF only thinks about efficiency too much, because gives priority to short processes
- If queue keeps building up, long job may never get CPU

#### Prediction of Next CPU Burst Time

- Don't tell who uses short and who uses long
- Cannot know in advance when entering CPU queue
- That is, because program doesn't proceed sequentially but situations differ each time like if statements
- Then how can we know next CPU burst time?
- If cannot know CPU burst, can estimate based on past CPU burst time

<br/>

## 7. Priority Scheduling

> A priority number (integer) is associated with each process

### Features

- Allocate CPU to process with highest priority (smallest integer)
- preemptive: take away and give if priority is higher
- nonpreemptive: don't take away, wait
- SJF is kind of priority scheduling

### Problems

- Starvation: low priority processes may never execute

### Solutions

- Aging: as time progresses increase the priority of the process
- Concept like seniority

<br/>

## 8. Round Robin

### Features

- Each process has same size allocation time (time quantum)
- When allocation time passes, process is preempted and goes to back of ready queue to line up again
- If n processes are in ready queue and allocation time is q time units, each process gets 1/n of CPU time in maximum q time unit units
- That is, no process waits more than (n-1) q time units

### Performance

- If allocation time is too long → FCFS
- If allocation time is too short → context switch occurs too frequently

<br/>

## 9. Multilevel Queue

> Divide ready queue into multiple

<p align="center" width="100%"><img width="1010" alt="image" src="https://github.com/ella-yschoi/TIL/assets/123397411/f801abd8-bfdd-4455-9d49-54465a7d7778">

### (1) Divide ready queue into multiple

- foreground: interactive
- background: batch - no human interaction

### (2) Each queue has independent scheduling algorithm

- foreground: Round Robin
- background: FCFS (long process executes continuously without context switch)
- Need scheduling for Queue (giving wait to each queue)

### (3) Fixed priority scheduling

- serve all from foreground then from background
- possibility of starvation

### (4) Time slice

- Allocate CPU time to each queue in appropriate ratio
- e.g. 80% to foreground in RR, 20% to background in FCFS
- Give priority to foreground but not unconditionally, give time weight to each queue

<br/>

## 10. Multilevel Feedback Queue

<p align="center" width="100%"><img width="1010" alt="image" src="https://github.com/ella-yschoi/TIL/assets/123397411/93ec9f6a-fee8-430c-977f-ac8bd1bf4eb0">

### (1) Features

- Movement between queues is possible
- Once decided queue doesn't change
- Process can move to different queue
- Higher queue has higher priority
- Lower queue fills only when higher queue is empty
- Can implement aging in this way

### (2) Parameters defining Multilevel Feedback Queue Scheduler

- Number of queues
- Scheduling algorithm of each queue
- Criteria for sending process to higher queue
- Criteria for sending process to lower queue
- Criteria for deciding which queue process enters when trying to receive CPU service

<br/>

## 11. Example of Multilevel Feedback Queue

### (1) Three queues

- Q0 - time quantum 8 milliseconds
- Q1 - time quantum 16 milliseconds
- Q2 - FCFS

### (2) Scheduling

- new job enters queue Q0
- If cannot finish within 8ms, goes down to queue Q1
- Lines up in Q1 and waits, then gets CPU and executes for 16ms
- If cannot finish within 16ms, gets evicted to queue Q2

<br/>

## 12. Multiple-Processor Scheduling

> When there are multiple CPUs, scheduling becomes more complex

### (1) In case of Homogeneous processor

- Can line up in one queue and let each processor take out by itself
- If there are processes that must execute on specific processor, problem becomes more complex

### (2) Load Sharing

- Need mechanism to appropriately share load so jobs don't crowd on some processors
- Method of having separate queues vs. method of using common queue

### (3) Symmetric Multiprocessing (SMP)

- Each processor decides scheduling by itself

### (4) Asymmetric Multiprocessing

- One processor is responsible for access and sharing of system data, others follow it

<br/>

## 13. CPU Scheduling in Special Cases

### (1) Load Balancing

- Basically Load Balancing when there are multiple CPUs
- Important for multiple CPUs to work evenly with balance
- Otherwise overall performance drops

### (2) SMP (Symmetric Multiprocessing)

- Not one master CPU but multiple CPUs are equal
- Therefore each processor decides scheduling by itself

### (3) ASMP (Asymmetric Multiprocessing)

- Method supplementing inefficient aspects of SMP
- One CPU becomes master CPU responsible for data access and sharing, other CPUs follow it

<br/>

## 14. Real-Time Scheduling

### (1) Hard real-time systems

- Must schedule to finish within fixed time
- For this, sometimes know each process's CPU arrival time in advance and schedule so system follows → Offline Scheduling
- Must leave CPU considering how much load each task has

### (2) Soft real-time computing

- Must have higher priority compared to general processes
- Good to guarantee deadline but expensive so mix with general processes
- e.g. Increase priority of video playback process so it can get CPU first to prevent video from cutting off

<br/>

## 15. Thread Scheduling

### (1) Thread

- Smallest execution unit running within process
- Each thread shares memory and resources with other processes running in same one process
- This structure facilitates communication between processes and enables efficient use of resources

### (2) Local Scheduling

- OS doesn't know thread's existence, user process itself has multiple threads inside so OS cannot make decision of which program to give CPU to
- Instead, user-level thread library (User level thread) is responsible for scheduling between threads
- These threads are managed at user level so can switch quickly without kernel intervention, but if one thread blocks, entire process can block

### (3) Global Scheduling

- Global scheduling occurs at kernel level, where OS can know thread's existence, and kernel scheduler decides which thread to execute
- This is possible because each thread is managed as separate kernel object
- That is, kernel's short-term scheduler decides which thread to schedule
- This method is efficient in multiprocessor systems but may cost more for context switch than user-level threads

### (4) Why Global Scheduling is Efficient in Multiprocessor Systems

> In multiprocessor systems, multiple CPU cores work simultaneously, and in this environment global scheduling provides following benefits

#### a. Flexibility in Resource Distribution

In global scheduling, kernel recognizes and manages all threads. This provides flexibility to allocate system's threads to various processor cores. Therefore, can maximize overall system performance by evenly distributing workload to multiple cores.

#### b. Efficient Load Balancing

In multiprocessor systems, can monitor each processor's load and move threads to other processors to evenly distribute load. This plays important role in improving system efficiency through load balancing.

#### c. Optimization of Parallel Processing

In multiprocessor systems, multiple threads can execute simultaneously. Can optimize such parallel processing through global scheduling, which is especially important in work with much parallel computation.

#### d. Deadlock and Starvation State Management

In multiprocessor environment, Deadlock and Starvation states increase complexity of process and thread management. Global scheduling provides overall perspective needed to manage and solve such problems.

#### e. Maintaining Synchronization and Consistency

In multiprocessor environment, synchronization of data and resources is important challenge. Through global scheduling, can maintain data consistency and effectively manage conflicts that may occur when accessing resources.

<br/>

## 16. Algorithm Evaluation

> Criteria for evaluating which CPU scheduling algorithm is good

### (1) Queueing models

- Calculate various performance index values through arrival rate and service rate given by probability distribution
- If requests pile up in queue when server processes requests, server processes, if not, remain in queue
- Requests wanting to use CPU arrive in queue, how fast frequency they arrive → this arrival rate is given by probability distribution
- CPU's performance or capability is called 'processing rate'
- After calculating complex formulas, Throughput and Waiting Time are derived

### (2) Implementation and Measurement

- Implement algorithm in actual system and measure and compare performance for actual workload

### (3) Simulation

- Write algorithm as simulation program then compare results using trace as input
- Trace means data that becomes Input, must be credible enough to be similar to or represent pattern of programs running in actual system
