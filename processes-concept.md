# Ch3 Processes Concept

**tags: Operating System**

## Process Concept <a id="Process-Concept"></a>

* **Program**  passive\(被動\) entity:binary stored in **disk**
* **Process**  active entity:a program in execution in **memory**

### A process includes: <a id="A-process-includes"></a>

* Code segment \(text section\)
* Data section — global variables
* Stack — temporary local variables and functions
* Heap — dynamic allocated variables or classes
* Current activity \(**program counter**, register contents\)
* A set of associated **resources**

### Process in Memory <a id="Process-in-Memory"></a>

![](https://i.imgur.com/Y91vB25.png)

### Threads\(線程\) <a id="Threads&#x7DDA;&#x7A0B;"></a>

* A.k.a lightweight process
* All threads belonging to the same **process** share  **code** section, **data** section, and **OS resources**
* But each thread has its own  **thread ID, program counter, register set, and a stack**  

![](https://i.imgur.com/fzDro4t.png)

### Process State <a id="Process-State"></a>

#### States <a id="States"></a>

* New\( 創建\)
* Ready
* Running
* Waiting
* Terminated \(終止\)    Only one process is **running** on any processor at any instant  However, many processes may be **ready or waiting**

![](https://i.imgur.com/YUwigmk.png)

#### Process Control Block \(PCB\) <a id="Process-Control-Block-PCB"></a>

過程控制塊（PCB）  
 ![](https://i.imgur.com/a35a5xK.png)

* **Process state**
* **Program counter**
* **CPU registers**
* CPU scheduling information
* Memory-management information
* I/O status information
* Accounting information

#### Context Switch <a id="Context-Switch"></a>

![](https://i.imgur.com/7MPQ9KS.png)

* **Context Switch**  Kernel saves the state of the old process and loads the saved state for the new process
* Context-switch time is purely **overhead**\(浪費\)
* Switch time \(about 1~1000 ms\)
  * memory speed
  * number of registers
  * existence of special instruction
  * hardware support  multiple sets of registers

## Process Scheduling \(排程\) <a id="Process-Scheduling-&#x6392;&#x7A0B;"></a>

* **Multiprogramming**  CPU runs process at all times to maximize CPU utilization
* **Time sharing**  switch CPU frequently such that users can interact with each program while it is running
* **overhead**

### Queues <a id="Queues"></a>

### Schedulers \(調度程序\) <a id="Schedulers-&#x8ABF;&#x5EA6;&#x7A0B;&#x5E8F;"></a>

![](https://i.imgur.com/5qJkvkH.png)

#### Short-term scheduler <a id="Short-term-scheduler"></a>

* CPU scheduler
* Ready state-&gt;Run state
* Execute quite frequently \(e.g.**once per 100ms**\)
* Must be efficient:  if 10 ms for picking a job, 100 ms for such a pick, **overhead** = 10 / 110 = 9%  

![](https://i.imgur.com/iZa5PEe.png)

#### Long-term scheduler <a id="Long-term-scheduler"></a>

* job scheduler
* New state-&gt;Ready state
* Control degree of multiprogramming
* Execute less frequently \(once several minutes\)
* Select a good mix of CPU-bound & I/O-bound processes to increase system overall performance
* UNIX/NT: no long-term scheduler

#### Medium-term scheduler <a id="Medium-term-scheduler"></a>

* Ready state-&gt;Wait state
* **swap out**  removing processes from memory
* **swap in**  reintroducing swap-out processes into memory
* improve process mix,free up memory  

![](https://i.imgur.com/CdGDQ5Q.png)

## Operations on Processes <a id="Operations-on-Processes"></a>

### Tree of Processes <a id="Tree-of-Processes"></a>

* Each process is identified by a unique processor identifier \(pid\)  ![](https://i.imgur.com/X2DUcvV.png)

#### Process Creation <a id="Process-Creation"></a>

* Resource sharing
* Two possibilities of execution
  * Parent and children **execute concurrently**
  * Parent **waits until children terminate**
* Two possibilities of address space
  * **Child duplicate of parent**, communication via sharing variables
  * **Child has a program loaded into it**, communication via message passing

**UNIX/Linux**

* fork system call
  * Create a new \(child\) process
  * The new process **duplicates**\(複製\) the address space of its parent
  * Child & Parent execute **concurrently** after fork
  * Child: return value of fork is 0
  * Parent: return value of fork is **PID** of the child process
* execlp system call
  * Load a new binary file into memory – **destroying** the old code
* wait system call
  * The parent waits for **one of its child processes** to complete
* Memory space of fork\(\)
  * Old:A’s child is an exact **copy** of parent
  * Current:use **copy-on-write** technique to store differences in A’s child address space  ![](https://i.imgur.com/aNHiV7I.png)
* Example

```cpp
#include <stdio.h>
void main( ){
    int A;
    /* fork another process */
    A = fork( );
    if (A == 0) { /* child process */
        printf(“this is from child process\n”);
        execlp(“/bin/ls”, “ls”, NULL);
    } else { /* parent process */
        printf(“this is from parent process\n”);
        int pid = wait(&status);
        printf(“Child %d completes”, pid);
    }
    printf(“process ends %d\n”, A);
}
```

```cpp
Output: 
this is from child process
this is from parent process
a.out hello.c readme.txt
Child 32185 completes
process ends 32185
```

![](https://i.imgur.com/5d0d3Jr.png)

### Termination <a id="Termination"></a>

* Terminate when the last statement is executed or **exit\(\)** is called
* arent may terminate execution of children processes by specifying its **PID \(abort\)**
* Cascading termination  killing \(exiting\) parent -&gt; killing \(exiting\) all its children

## Interprocess Communication\(IPC\) <a id="Interprocess-CommunicationIPC"></a>

* IPC: a set of methods for the exchange of data among multiple threads in one or more processes
* Independent process
* Cooperating process:

### Communication Methods <a id="Communication-Methods"></a>

![](https://i.imgur.com/UabK8io.png)

### Shared memory <a id="Shared-memory"></a>

* Require more careful **user synchronization**\(同步\)
* Use memory address to access data,**faster speed**
* Consumer & Producer Problem
  * Producer process produces information that is consumed by a Consumer process

```cpp
/*producer*/
while (1) {
    while (((in + 1) % BUFFER_SIZE) == out)
        ; //wait if buffer is full
    buffer[in] = nextProduced;
    in = (in + 1) % BUFFER_SIZE;
}
```

![](https://i.imgur.com/W9whvgI.png)

```cpp
/*consumer*/
while (1) {
    while (in == out); //wait if buffer is empty
    nextConsumed = buffer[out];
    out = (out + 1) % BUFFER_SIZE;
}
```

![](https://i.imgur.com/W0Pv5N2.png)

### Message passing <a id="Message-passing"></a>

* No conflict: **more efficient for small data**
* Use send/recv message
* Implemented by **system call**
* Mechanism for processes to communicate and **synchronize** their actions

#### Direct communication\(直接\) <a id="Direct-communication&#x76F4;&#x63A5;"></a>

* Processes must name each other explicitly
  * Send \(P, message\)
  * Receive \(Q, message\)
* Properties
  * Links are **established automatically**
  * **One-to-One**
* limited modularity

```cpp
/*producer*/
while (1) {
    send (consumer, nextProduced);
}

/*consumer*/
while (1) {
    receive (producer, nextConsumed);
}
```

#### Indirect communication\(間接\) <a id="Indirect-communication&#x9593;&#x63A5;"></a>

* Messages are directed and received from ports
  * **Each mailbox has a unique ID**
  * Send \(A, message\)
  * Receive \(A, message\)
* Properties
  * Link established only if **processes share a common mailbox**
  * **Many-to-Many**  
* Allow a link to be associated with at most two processes
* Allow only one process at a time to execute a receive operation
* Allow the system to select arbitrarily a single receiver. Sender is notified who the receiver was

![](https://i.imgur.com/5AyP01M.png)

#### Synchronization <a id="Synchronization"></a>

* Message passing may be either blocking \(synchronous\) or non-blocking \(asynchronous\)
  * Blocking send
  * Nonblocking send
  * Blocking receive
  * Nonblocking receive  
* Buffer implementation
  * **Zero** capacity\(容量\):blocking send/receive
  * **Bounded** capacity:if full, sender will be blocked
  * **Unbounded**capacity: sender never blocks

![](https://i.imgur.com/yRBFP05.png)

### Sockets <a id="Sockets"></a>

### Remote\(遠端\) Procedure Calls \(RPC\) <a id="Remote&#x9060;&#x7AEF;-Procedure-Calls-RPC"></a>

* Cause a **procedure** to execute in another address space  
* Abstracts procedure calls between processes\*\* on networked systems\*\*

![](https://i.imgur.com/kS5IAIQ.png)

#### Stubs <a id="Stubs"></a>

## Backup <a id="Backup"></a>

### POSIX Shared Memory <a id="POSIX-Shared-Memory"></a>

```cpp
{
/* allocate a R/W shared memory segment */
char* segment_id = shmget(IPC_PRIVATE, 4096, S_IRUSR | S_IWUSR);
/* attach the shared memory segment */
char* shared_memory = (char*) shmat(segment_id, NULL, 0);
/* write a message to the shared memory segment */
sprintf(shared_memory, “Write to shared memory”);
/* print out the string from the shared memory segment */
printf(“%s\n”, shared_memory);
/* detach the shared memory segment */
shmdt(shared_memory);
/* remove the shared memory segment */
shmctl(shared_memory, IPC_RMID, NULL);
}
```

### Mach Message Passing <a id="Mach-Message-Passing"></a>

* Mach operating system
  * developed at CMU
  * **microkernel** design
  * most communications are carried out by messages and mailboxes \(aka ports\)
  * Problem: performance \(data coping\)
* When each task \(process\) is created
  * **kernel & notify** mailboxes also created

### Mach Mailbox <a id="Mach-Mailbox"></a>

port-allocate: system call to create a mailbox

* default buffer size: **8 messages**
* FIFO queueing
* message: one fixed-size header + variable-length data portion  

![](https://i.imgur.com/CqCwgvD.png)

### RPC Problems <a id="RPC-Problems"></a>

* Data Representation Issue
  * **External data representation \(XDR\)** \(外部數據表示\)
* Address Space Issue
  * No pointer usage in RPC calls
  * Copy the entire pointed area
* Communication Issue
  * at most once: **timestamp**
  * exact once:
    * The server must acknowledge to the client
    * The client must resend each RPC call periodically

### Pipes <a id="Pipes"></a>

* Pipe is a special type of **file**  

![](https://i.imgur.com/xvyYynM.png)

#### Ordinary Pipes <a id="Ordinary-Pipes"></a>

* Requires a parent-child relationship between the communicating processes
* Unidirectional

#### Named Pipes <a id="Named-Pipes"></a>

* **No parent-child** relationship is required
* Several processes can use it for communications
* Continue to exist after communicating processes **exit**

### Remote Method Invocation <a id="Remote-Method-Invocation"></a>

* RMI is a Java mechanism similar to RPC
* RMI allows a Java program on one machine to invoke a methodi on a remote object instead of a functioni 

