# Ch1 Introduction

**tags: Operating System**

## What is an Operating System <a id="What-is-an-Operating-System"></a>

### Computer System <a id="Computer-System"></a>

![](https://i.imgur.com/dUD1EUo.png)

* User
* Application  define the ways in which the system resources are used to solve the computing problems
* Operating System  ontrols and coordinates the use of the **hardware/resources**
* Hardware  provides basic **computing resources**

### What is an Operating System? <a id="What-is-an-Operating-System1"></a>

An operating system is the **“permanent”** software that controls/abstracts hardware resources for user applications  
 

![](https://i.imgur.com/e7iixbw.png)

### Multi-tasking Operating Systems <a id="Multi-tasking-Operating-Systems"></a>

* Manages resources and processes to support different user applications
* Provides Applications Programming Interface \(API\) for user applications  

![](https://i.imgur.com/ZSGBUsb.png)

### General-Purpose Operating Systems <a id="General-Purpose-Operating-Systems"></a>

![](https://i.imgur.com/GFEOn0e.png)

### Definition <a id="Definition"></a>

* Resource allocator \(資源分配器\)  manages and allocates resources to insure efficiency and fairness \(管理和分配資源以確保效率和公平性\)
* Control program  controls the execution of user programs and operations of I/O devices to prevent errors and improper use of computer
* Kernel  the one program running at all times
* **No universally accepted definition**

### Goals <a id="Goals"></a>

* Convenience
* Efficiency
* Two goals are sometimes contradictory \(矛盾\)

### Importance <a id="Importance"></a>

* System API are the **only** interface between user applications and hardware
* OS code cannot allow any bug
* The owner of OS technology **controls** the software & hardware industry
* Operating systems and computer architecture influence each other

### Modern Operating Systems <a id="Modern-Operating-Systems"></a>

* x86 platform
* PowerPC platform – Mac OS
* Smartphone Mobile OS
* Embedded OS

## Computer-System Organization <a id="Computer-System-Organization"></a>

One or more CPUs, device controllers connect through common bus providing access to shared memory  
 

![](https://i.imgur.com/Q0Yrdxh.png)

### Goal <a id="Goal"></a>

Concurrent execution of CPUs and devices competing for memory cycles

### Operations <a id="Operations"></a>

* Each device controller has a local buffer
* I/O is from the device to controller’s local buffer
* CPU moves datafrom/to memory to/from local buffers in device controllers  ![](https://i.imgur.com/H5c0ViH.png)

### Busy/wait output <a id="Busywait-output"></a>

```cpp
#define OUT_CHAR 0x1000 // device data register
#define OUT_STATUS 0x1001 // device status register

current_char = mystring;
while (*current_char != ‘\0’) {
    poke(OUT_CHAR,*current_char);
    while (peek(OUT_STATUS) != 0); // busy waiting
    current_char++;
}
```

### Interrupt I/O\(中斷\) <a id="Interrupt-IO&#x4E2D;&#x65B7;"></a>

### Common Functions of Interrupts <a id="Common-Functions-of-Interrupts"></a>

* Interrupt transfers control to the interrupt service routine, through the interrupt vector, which contains the **addresses** of all the service routines.
* Interrupt architecture must save the address of the interrupted instruction
* Incoming interrupts are disabled while another interrupt is being processed to prevent a **lost interrupt**

### Storage-Device Hierarchy \(存儲設備層次結構\) <a id="Storage-Device-Hierarchy-&#x5B58;&#x5132;&#x8A2D;&#x5099;&#x5C64;&#x6B21;&#x7D50;&#x69CB;"></a>

![](https://i.imgur.com/PThTX24.png)

* Speed,Cost,Volatility
* **Main memory**  only large storage media that the CPU can access directly
* **Secondary storage**  extension of main memory that provideslarge nonvolatile storage capacity

#### RAM: Random-Access Memory <a id="RAM-Random-Access-Memory"></a>

* DRAM \(Dynamic RAM\):
  * Need only \*_one transistor_ \(電晶體\)
  * Consume **less power**
  * values must be periodically **refreshed**
  * Access Speed: **&gt;= 30ns**
* SRAM \(Static RAM\):
  * Need **six transistors**
  * Consume **more power**
  * Access Speed: **10ns~30ns**
  * usage: **cache memory** \(緩存\)

#### Disk Mechanism <a id="Disk-Mechanism"></a>

* Speed of magnetic disk
  * Transfer time = data size / transfer rate
  * Positioning time \(random access time\)  ![](https://i.imgur.com/CLDgLeU.png)

### Caching \(快取\) <a id="Caching-&#x5FEB;&#x53D6;"></a>

### Coherency and Consistency Issue <a id="Coherency-and-Consistency-Issue"></a>

* **The same data may appear in different levels**
* Single task accessing  use the Highest level copy
* Multi-task accessing  ![](https://i.imgur.com/5eAiuEU.png)

## Hardware Protection <a id="Hardware-Protection"></a>

### Dual-Mode Operation <a id="Dual-Mode-Operation"></a>

* Provide hardware support to differentiate between at least two modes of operations
  1. User mode
  2. Monitor mode \(監控\)  \(**kernel mode or system mode**\)  execution done on behalf of operating system  ![](https://i.imgur.com/C4ASAOx.png)
* Privileged instructions \(特權指令\)
  * Executed only in **monitor mode**
  * Requested by users \(system calls\)

### I/O Protection <a id="IO-Protection"></a>

* All I/O instructions are privileged instructions
* Must ensure that a user program could never gain control of the computer in monitor mode  

![](https://i.imgur.com/RpAjeIX.png)

### Memory Protection <a id="Memory-Protection"></a>

### CPU Protection <a id="CPU-Protection"></a>

* Prevent user program from not returning control
  * getting stuck in an infinite loop
  * not calling system services
* HW support  Timer - interrupts computer after specified period
* Timer commonly used to implement **time sharing**
* **Load-timer** is a privileged instruction

