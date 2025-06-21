# Process Synchronization

## 1. Computer Operations

<p align="left" width="100%"><img width="1000" alt="Computer Operations" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/42000663-94c1-4bc0-b8b7-75f6e1840946">

- When a computer performs an operation, it always reads data, performs the operation, and then stores the result somewhere again.
- Data in memory is read, operated on by the CPU, and then brought back to memory for use.
- The operation is not performed in place, but something is brought in, processed, and then sent back out again.

<br/>

## 2. Race Condition

<p align="left" width="100%"><img width="1000" alt="Race Condition" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/1820b341-e4e6-4387-93a4-5d9432b601d8">

### (1) Problem Occurrence

- Usually occurs when data is read not by one entity, but by several entities.
- For example, if the value of the count variable is increased on the left and decreased on the right, the count value should be stored as the original value.
- However, if two entities operate on the same data at the same time, a problem can occur when one reads the data "in the middle" of the other's operation.
- When multiple entities try to access shared data at the same time → Race Condition.

### (2) Is it always a problem?

- If there are multiple CPUs, a race condition can occur, but it doesn't always happen.
- Even with a single CPU, a race condition can still occur depending on how the OS schedules processes.

### (3) How the Problem Occurs

> Even with a single CPU, if the OS switches between processes, a race condition can occur.

- For example, if program A is running and needs the OS to do something it can't do itself, it requests a system call.
- While process A is holding the CPU and working, it makes a system call for something it can't do itself. At this point, A is in the middle of changing data in the OS, but its CPU time ends.
- The CPU switches from A to B. B now holds the CPU and also makes a system call for something it can't do itself. Now, the OS code is executed for B's request.
- If the OS code changes the same data that A was changing, both A and B can end up modifying the same data.
- When the CPU returns to A, the operation that A was doing may have already been affected by B's changes, leading to a race condition.

### (4) Summary: When Race Condition Occurs in the OS

1. When an interrupt occurs during kernel execution
2. When a process makes a system call and a context switch occurs during kernel mode
3. When multiple processors access shared memory in the kernel

## 3. Race Condition in the OS

### (1) How the Problem Occurs

<p align="left" width="100%"><img width="1000" alt="Race Condition" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/a0f9ae90-96b3-483b-9bff-03a4ae465d4f">

- When program A is executing its own code in user mode, it only accesses its own address space, so there is no problem.
- However, when a system call is made and kernel code is executed, kernel data is accessed, and from the process's perspective, kernel data is a type of **shared data**.
- Therefore, if a program alternates between user mode and kernel mode, there is no problem in user mode, but if the CPU is taken away while modifying kernel data in kernel mode, a race condition can occur.

### (2) Solution

<p align="left" width="100%"><img width="1000" alt="Race Condition Solution" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/58de6d59-3832-4795-941b-eb0bdd631d7c">

- Do not preempt (take away) the CPU while in kernel mode.
- Only preempt when returning from kernel mode to user mode.

### (3) Race Condition in the OS (interrupt handler vs. kernel)

<p align="left" width="100%"><img width="1000" alt="OS Race Condition (interrupt handler vs. kernel)" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/b09ed6ff-37d5-42a5-821b-105ac1ea6e36">

- When an interrupt occurs, the current context is saved, the interrupt handler is executed, and then the previous work resumes.
- To prevent issues, interrupts can be disabled before modifying variables and re-enabled after the work is done (unless it's a real-time system).
- Even though processes do not share address spaces, race conditions can still occur when working inside the OS.

### (4) Race Condition in the OS (multi-processor)

<p align="left" width="100%"><img width="1000" alt="OS Race Condition (multi-processor)" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/51aff8e3-47f0-482c-acd1-457f788f0d8b">

#### a. Method 1

- Lock the entire OS so only one CPU can enter the kernel at a time.
- This means the entire kernel is locked, and no other CPU can use it simultaneously.

#### b. Method 2

- Lock individual data within the OS.
- As long as a particular data item is not being accessed, other operations can proceed concurrently.
- Each time shared data in the kernel is accessed, lock/unlock that specific data.

## 4. The Process Synchronization Problem

> Concurrent access to shared data can cause data inconsistency problems.

### (1) Race Condition

- When multiple processes access shared data at the same time.
- The final result of the data depends on which process last handled it.
- For example, if process A does count++ and process B does count--, the result should be 0, but due to race conditions, the result may be inconsistent.
- To prevent race conditions, concurrent processes must be synchronized.

### (2) Solution

- To maintain consistency, a mechanism is needed to enforce orderly execution among cooperating processes.
- The idea is to allow a process to finish modifying data before another process can access it (A → B or B → A, etc.).
- By enforcing execution order during cooperation, we prevent the problem of a process being preempted "in the middle" of modifying data.

## 5. Example of a Race Condition

<p align="left" width="100%"><img width="1000" alt="Example of a Race Condition" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/0de20292-283c-4ea5-b701-dc9fee819b8e">

- If a timer interrupt occurs while user process P1 is running, causing a context switch to P2, the problem is that the CPU can be switched to another process "in the middle" of an operation.
- The code that accesses shared data is called the critical section.

<br/>

## 6. The Critical-Section Problem

<p align="left" width="100%"><img width="1000" alt="The Critical-Section Problem" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/61854ede-f797-4ee6-89b0-bde9acafdf43">

- When n processes want to use shared data at the same time.
- Each process's code segment contains a critical section, which is the code that accesses shared data (the shared data itself is not the critical section; the code that accesses it is).
- Problem: When one process is in its critical section, no other process should be allowed to enter its critical section. In other words, before entering the critical section, a lock must be acquired, and the lock must be released after exiting.

## 7. Attempts to Solve the Problem

### (1) Initial Attempts to Solve the Problem

<p align="left" width="100%"><img width="1000" alt="Initial Attempts to solve problem" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/b8fe2787-ee3b-42bf-889c-f99b4b15402f">

- Since problems occur when multiple processes enter the critical section at the same time, entry and exit code can be added to prevent simultaneous access.

### (2) Requirements for Programmatic Solutions

> Assumption 1. The execution speed of all processes is greater than 0.<br/>
> Assumption 2. No assumptions are made about the relative execution speed of processes.

#### a. Mutual Exclusion

- The condition that no two processes can be in their critical sections at the same time.
- If process Pi is executing in its critical section, no other process can be in its critical section.

#### b. Progress

- The condition that a process wanting to enter the critical section should not be prevented from doing so.
- If no process is in the critical section and some processes want to enter, one of them should be allowed to enter.

#### c. Bounded Waiting

- The condition that waiting time must be finite (to prevent starvation).
- After a process requests entry to the critical section, there must be a limit on the number of times other processes can enter before it is allowed to enter.

### (3) Algorithm 1 to Prevent Simultaneous Access

<p align="left" width="100%"><img width="1000" alt="Algorithm 1 to prevent simultaneous access" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/0987a3a2-c218-4245-85e4-1c56bff58db6">

- If it's not your turn, you keep waiting in a while loop. If turn is 0 (your turn), you enter the critical section. When done, you set turn to 1.
- Before entering the critical section, check if it's your turn. If not, wait; if yes, enter and execute the shared code, then set the turn for the other process when done.
- This prevents both from entering the critical section at the same time, but strictly alternates access, so if one wants to enter multiple times and the other doesn't, the first can't enter repeatedly (overly strict alternation).

### (4) Algorithm 2 to Prevent Simultaneous Access

<p align="left" width="100%"><img width="1000" alt="Algorithm 2 to prevent simultaneous access" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/03ad06e8-9ddc-4a73-81f8-b5e87273f724">

- The flag algorithm: each process has its own flag. If a process wants to enter the critical section, it raises its flag.
- A process signals its intention to enter: flag[i] = true; then checks if the other process's flag is down. If the other flag is up, it doesn't enter; if down, it enters the critical section. When done, it lowers its flag: flag[i] = false;
- This prevents simultaneous entry, but if both raise their flags and wait, neither can enter (progress condition violation).

### (5) Algorithm 3 to Prevent Simultaneous Access

<p align="left" width="100%"><img width="1000" alt="Algorithm 3 to prevent simultaneous access" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/6c633233-3e53-4d15-b02a-4a17248f8c44">

- Uses both flags and turn variable. If both raise their flags at the same time, turn decides whose turn it is.
- Before entering the critical section: flag[i] = true; set turn = j; then check if the other flag is up and turn is the other's in a while loop; when done, lower your flag: flag[i] = false;
- This still has the problem of busy waiting (spin lock): if stuck in the while loop, the process keeps using CPU and memory while waiting.

### (6) Synchronization Hardware

<p align="left" width="100%"><img width="1000" alt="Synchronization Hardware" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/e579ad52-894b-4605-b3d7-b544ff9d837c">

- Hardware can support atomic operations, so that reading and writing a variable is done "all at once" without being interrupted. If Test and Set is supported, the above complex algorithms are not needed.
- For example, read variable a and set it to 1 in one atomic operation.

## 8. Semaphores

### (1) What is a Semaphore?

> For an easy explanation of the semaphore concept: [Difference between Mutex and Semaphore](https://worthpreading.tistory.com/90)

<p align="left" width="100%"><img width="1000" alt="Semaphore" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/87875110-ad0b-4598-8eb4-f25117e214e0">
