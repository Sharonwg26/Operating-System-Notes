# Ch4 Multithreaded Programming

**tags: Operating System**

## Thread Introduction <a id="Thread-Introduction"></a>

### Threads\(線程\) <a id="Threads&#x7DDA;&#x7A0B;"></a>

* A.k.a lightweight process
* All threads belonging to the same **process** share  **code** section, **data** section, and **OS resources**
* But each thread has its own  **thread ID, program counter, register set, and a stack**  

![](https://i.imgur.com/fzDro4t.png)

### Motivation <a id="Motivation"></a>

#### RPC server <a id="RPC-server"></a>

![](https://i.imgur.com/cZCQJiu.png)

### Benefits of Multithreading <a id="Benefits-of-Multithreading"></a>

* Responsiveness\(反應速度\)
* Resource sharing  several different threads of activity all within the same address space
* Utilization of MP arch.  Several thread may be running in paralle on different processors
* Economy

### Multithcore Programming \(多核編程\) <a id="Multithcore-Programming-&#x591A;&#x6838;&#x7DE8;&#x7A0B;"></a>

* More efficient use of multiple core\(核心\) and improved concurrency\(並發\)
* Multicore systems putting pressure on system designers and application programmers  

![](https://i.imgur.com/zWAa9EB.png)

#### Challenges <a id="Challenges"></a>

* Dividing activities
* Balance
* Data splitting
* Data dependency
* Testing and debugging

### User vs. Kernel Threads <a id="User-vs-Kernel-Threads"></a>

#### User threads <a id="User-threads"></a>

* thread management done by user-level **threads library**
  * Win32
  * Java
* Thread library provides support for thread creation, scheduling, and deletion
* fast
* If the kernel is single-threaded, a user-thread blocks -&gt; entire process blocks even if other threads are ready to run

#### Kernel threads <a id="Kernel-threads"></a>

* Supported by the **kernel \(OS\)** directly
  * Windows 2000 \(NT\)
  * Linux
* The **kernel** performs thread creation, scheduling
* slower
* If a thread is blocked, the kernel can **schedule another thread** for execution

## Multithreading Models <a id="Multithreading-Models"></a>

### Many-to-One <a id="Many-to-One"></a>

![](https://i.imgur.com/NqmMDO7.png)

* Many user-level threads mapped to single kernel thread
* Thread management is done in user space, so it is efficient
* ![:-1:](https://cdn.jsdelivr.net/npm/@hackmd/emojify.js@2.1.0/dist/images/basic/-1.png)
  * The entire process will block if a thread makes a blocking system call
  * multiple threads are unable to run in parallel on multiprocessors,

### One-to-One <a id="One-to-One"></a>

![](https://i.imgur.com/43sBI9T.png)

### Many-to-Many <a id="Many-to-Many"></a>

![](https://i.imgur.com/lx4zZ8S.png)

* Allows the developer to create as many user **threads** as wished
* ![:+1:](https://cdn.jsdelivr.net/npm/@hackmd/emojify.js@2.1.0/dist/images/basic/+1.png)
  * The corresponding kernel threads can run in parallel on a multiprocessor
  * When a thread performs a blocking call, the kernel can **schedule\(安排\)** another thread for execution.

## Threaded Case Study <a id="Threaded-Case-Study"></a>

### Thread libraries <a id="Thread-libraries"></a>

#### Shared-Memory Programming <a id="Shared-Memory-Programming"></a>

* through a shared memory space
* **Faster & more efficient** than message passing
* techniques
  * Parallelizing compiler
  * Unix processes
  * Threads
* issues
  * Synchronization
  * Deadlock
  * Cache coherence  

![](https://i.imgur.com/Mqq5BJa.png)

#### Pthreads <a id="Pthreads"></a>

* **POSIX \(Potable Operating System Interface\)** standard  specified forportability across Unix-like systems
* Pthread is the implementation of POSIX standard for thread

**Creation**

* pthread\_create\(thread,attr,routine,arg\)
  * thread：An unique identifier \(token\) for the new thread
  * attr: It is used to set thread attributes. **NULL** for the default value
  * routine: The routine that the thread will execute once it is created
  * arg: A single argument that may be passed to routine  

![](https://i.imgur.com/Co0Tek8.png)

```cpp
#include <pthread.h>
#include <stdio.h>
#define NUM_THREADS 5

void *PrintHello(void *threadId) {
    long* data = static_cast <long*> threadId;
    printf("Hello World! It's me, thread #%ld!\n", *data);
    pthread_exit(NULL);
}
int main (int argc, char *argv[]) {
    pthread_t threads[NUM_THREADS];
    for(long tid=0; tid<NUM_THREADS; tid++){
        pthread_create(&threads[tid], NULL, PrintHello, (void *)&tid);
    }
    /* Last thing that main() should do */
    pthread_exit(NULL);
}
```

**Joining & Detaching**

* pthread\_join\(threadId, status\)
  * Blocks until the specified **threadId** thread terminates
  * One way to **accomplish synchronization**
  * `for (int i=0; i<n; i++) pthread_join(thread[i], NULL);`
* pthread\_detach\(threadId\)
  * Once a thread is **detached**, it can **never** be joined
  * Detach a thread could free some system resources  

![](https://i.imgur.com/mTMv7Ix.png)

#### Java Threads <a id="Java-Threads"></a>

* Created
  * Extending Thread class
  * Implementing the Runnable interface
* Thread library on the **host system**
* Thread mapping depends on implementation of the **JVM**
  * Windows 98/NT: one-on-one model
  * Solaris 2: many-to-many model

#### Linux Threads <a id="Linux-Threads"></a>

* Linux does not support multithreading
* Vrious **Pthreads** implementation are available for user-level
* The fork system call  create a new process and a copy of the associated data of the parent process
* The clone system call
  * * create a new process and a link that points to the associated data of the parent process
  * None of the flags is set -&gt; clone = fork
  * All flags are set -&gt; parent and child share everything  

![](https://i.imgur.com/OYcydtN.png)

#### Windows XP Threads <a id="Windows-XP-Threads"></a>

* Implement the one-to-one mapping
* Each thread contains
  * A thread ID
  * Register set
  * Separate user and kernel stacks
  * Private data storage area
* The primary data structures of a thread include:
  * ETHREAD \(executive thread block\)
  * KTHREAD \(kernel thread block\)
  * TEB \(thread environment block\)
* Also provide support for a fiber library, that provides the functionality of the many-to-many model  

![](https://i.imgur.com/ThWe1JS.png)

## Threading Issues <a id="Threading-Issues"></a>

### Semantics\(語義\) of fork\(\) and exec\(\) <a id="Semantics&#x8A9E;&#x7FA9;-of-fork-and-exec"></a>

* Some UNIX system support **two versions of** fork\(\)
* **execlp\(\)** works the same; replace the entire process  

![](https://i.imgur.com/Qs4QLRw.png)

### Thread Cancellation\(取消\) <a id="Thread-Cancellation&#x53D6;&#x6D88;"></a>

* Target thread: a thread that is to be cancelled
* Two general approaches
  * **Asynchronous cancellation**
  * **Deferred cancellation \(default option\)**延遲取消（默認選項）
    * Check at **Cancellation** point

### Signal Handling\(信號\) <a id="Signal-Handling&#x4FE1;&#x865F;"></a>

* Signals \(synchronous or asynchronous\) are used in UNIX systems to notify a process that an event has occurred
  * Synchronous: illegal memory access
  * Asynchronous: `<control-C>`
* Signal handler
  1. Signal is **generated** by particular event
  2. Signal is **delivered** to a process
  3. Signal is **handled**
* Options
  * Deliver the signal to the thread to which the signal applies
  * Deliver the signal to every thread in the process
  * Deliver the signal to certain threads in the process
  * Assign a specific thread to receive all signals for the process

### Thread Pools <a id="Thread-Pools"></a>

* Create a number of threads in a pool where they **await work**
* Advantages
  * Slightly **faster** to service a request with an **existing thread** than create a new thread
  * Allows the number of threads in the application\(s\) to be bound to the size of the pool
  * \# of threads: \# of CPUs, expected \# of requests, amount of physical memory

### Thread Specific Data <a id="Thread-Specific-Data"></a>

* Allows each thread to have its own **copy of data**
* Useful when you do not have control over the thread creation process

### Scheduler Activations <a id="Scheduler-Activations"></a>

* Scheduler activations provide **upcalls** - a communication mechanism from the kernel to the thread library
* This communication allows an application to maintain the correct number kernel threads

