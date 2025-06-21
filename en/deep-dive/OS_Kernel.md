# OS Kernel

![kernal.png](/Images/kernal.png)

## Main Characteristics of the Kernel

- The computer's kernel is the `core` part of the operating system.
- It is responsible for `managing and controlling communication between hardware and software`.
- The kernel is located at the `highest level` of the operating system.
- It `controls the basic operations` of the system and manages various hardware and software resources.

## Types of Kernel

1. **Monolithic**

   - Most system functions are handled in a `single kernel`.

2. **Microkernel**
   - Only basic functions are handled in the kernel, and `other functions are executed in user mode`.

## Main Functions of the Kernel

1. **System Resource Management**

   - The kernel manages system hardware resources.
   - This includes CPU time, memory, and input/output devices.
   - It allocates and releases memory and CPU time for each process and thread, so that `programs can use as much memory as needed`.

2. **Hardware Abstraction**

   - The kernel acts as an intermediary between hardware and software.
   - It provides a unified interface for various hardware devices.
   - Applications can operate in a `consistent way` without knowing the details of each hardware device.

3. **System Call Interface**

   - Applications invoke kernel functions to `access hardware resources or perform various operations`.
   - This interface is called a system call, and it mediates interaction between applications and the kernel.

4. **Security and Access Control**

   - The kernel manages access rights to system resources and data.
   - It maintains separation between users and applications, strengthens security, and `protects the system from illegal access and malicious actions`.

5. **Interrupt Handling**
   - Communication between hardware and software is mediated by a mechanism called interrupts.
   - Interrupts notify the kernel of unexpected situations or events.
   - Through this, the `system can handle external input or device operations in a timely manner`.
