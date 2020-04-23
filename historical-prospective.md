# Ch0 Historical Prospective

**tags: Operating System**

## Mainframe Systems\(大型\) <a id="Mainframe-Systems&#x5927;&#x578B;"></a>

1. Batch→Multi-programming→Time-shared
2. For critical application with better **reliability & security**
3. Bulk\(批量\) data processing

### Batch Systems <a id="Batch-Systems"></a>

![](https://i.imgur.com/LIgNtVr.png)

#### Processing steps <a id="Processing-steps"></a>

* Users submit jobs \(program, data, control card\)
* Operator sort jobs with similar requirements
* OS simply transfer control from one job to the next

#### Drawbacks \(缺點\) <a id="Drawbacks-&#x7F3A;&#x9EDE;"></a>

* One job at a time
* No interaction between user and jobs
* CPU is often idle
* OS doesn’t need to make any decision

### Multi-programming Systems <a id="Multi-programming-Systems"></a>

![](https://i.imgur.com/NizpXIN.png)

#### Overlaps the I/O and computation of jobs <a id="Overlaps-the-IO-and-computation-of-jobs"></a>

#### Spooling \(Simultaneous Peripheral Operation On-Line\) <a id="Spooling-Simultaneous-Peripheral-Operation-On-Line"></a>

* I/O is done with no CPU intervention
* CPU just needs to be notified when I/O is done  

![](https://i.imgur.com/nqPBrZl.png)

#### OS tasks <a id="OS-tasks"></a>

* Memory management– the system must allocate the memory to several jobs
* CPU scheduling \(調度\) – the system must choose among several jobs ready to run.
* I/O system – I/O routine supplied by the system, allocation of devices

### Time-sharing System\(Multi-tasking System\) <a id="Time-sharing-SystemMulti-tasking-System"></a>

#### An interactive system provides direct \(直接\) communication between the users and the system <a id="An-interactive-system-provides-direct-&#x76F4;&#x63A5;-communication-between-the-users-and-the-system"></a>

#### Switch job when <a id="Switch-job-when"></a>

* finish
* waiting I/O
* short period of time

#### OS tasks <a id="OS-tasks1"></a>

* Virtual memory – jobs swap in and out of memory to obtain reasonable response time
* File system and disk management – manage files and disk storage for user data
* Process synchronization and deadlock – support concurrent execution of programs

|  | Batch | Multi-programming | Time-sharing |
| :--- | :--- | :--- | :--- |
| System Model | Single user 、Single job | Multiple prog. | Multiple users、Multiple prog. |
| Purpos\(目的\) | Simple | Resource utilization\(資源利用率\) |  Interactive、Response time |
| OS features | N.A | CPU scheduling、Memory Mgt.、I/O system | File system、Virtual memory、Synchronization、Deadlock |

## Computer-system architecture <a id="Computer-system-architecture"></a>

### Desktop Systems: Personal Computers <a id="Desktop-Systems-Personal-Computers"></a>

* Personal computers \(PC\)
* User convenience and responsiveness – GUI
* I/O devices
* Several different types of operating systems
* Lack of file and OS protection from users

### Parallel Systems: tightly coupled <a id="Parallel-Systems-tightly-coupled"></a>

\(平行系統：緊密聯結\)

* A.k.a multiprocessor or tightly coupled system  Usually communicate through shared memory
* Throughput,Economical,Reliability  ![](https://i.imgur.com/Jfu8faS.png)

#### Symmetric multiprocessor system \(SMP\) <a id="Symmetric-multiprocessor-system-SMP"></a>

\(對稱多處理器系統（SMP）\)

* Each processor runs the same OS
* Require extensive synchronization\(同步\) to protect data integrity

#### Asymmetric multiprocessor system <a id="Asymmetric-multiprocessor-system"></a>

\(非對稱多處理器系統\)

* Each processor is assigned a specific task
* One Master CPU & multiple slave CPUs

#### Multi-Core Processor <a id="Multi-Core-Processor"></a>

* A CPU with multiple cores on the same die \(chip\)
* On-chip communication is faster than between-chip communication  

![](https://i.imgur.com/inJbVdm.png)

#### Memory Access Architecture <a id="Memory-Access-Architecture"></a>

\(內存訪問架構\)

### Distributed Systems: loosely coupled <a id="Distributed-Systems-loosely-coupled"></a>

\(分佈式系統：鬆散聯結\)

* A.k.a loosely coupled system
  * Each processor has its own **local memory**
  * Easy to scale to large number of nodes \(e.g. Internet\)
* Resource sharing,Load sharing,Reliability

#### Client-Server Distributed System <a id="Client-Server-Distributed-System"></a>

* Easier to manage and control resources
* Server becomes the bottleneck and single failure point  ![](https://i.imgur.com/tYvzdr5.png)

#### Peer-to-Peer Distributed System <a id="Peer-to-Peer-Distributed-System"></a>

* Every machine is identical in its role in the distributed system – decentralized  ![](https://i.imgur.com/Bk4J1J4.png)

#### Clustered Systems <a id="Clustered-Systems"></a>

\(集群系統\)

* Definition  Cluster computers share storage and are closely linked via a local area network \(LAN\) or a faster interconnect, such as InfiniBand.
* Asymmetric clustering
* Symmetric clustering

#### System Architecture Summary <a id="System-Architecture-Summary"></a>

![](https://i.imgur.com/L5ORMxu.png)

## Special-purpose Systems\(專用\) <a id="Special-purpose-Systems&#x5C08;&#x7528;"></a>

### Real-Time Systems <a id="Real-Time-Systems"></a>

* Defined
  * fixed-time constraints\(約束時間\)
  * keeping deadlines
* Guaranteed response and reaction times
* Real-time requirement: hard or soft

#### Soft vs. Hard Real-Time <a id="Soft-vs-Hard-Real-Time"></a>

**Soft real-time**

A critical real-time task gets **priority** over other tasks, and retains that priority until it completes

**Hard real-time**

* Missing the deadline results in a fundamental failure
* Secondary storage limited or absent, data stored in short term memory, or read-only memory \(ROM\)

### Multimedia Systems\(多媒體系統\) <a id="Multimedia-Systems&#x591A;&#x5A92;&#x9AD4;&#x7CFB;&#x7D71;"></a>

* A wide range of applications including audio and video files
* Issues
  * Timing constraints: 24~30 frames per second
  * On-demand/live streaming: media file is only played but not stored
  * Compression: due to the size and rate of multimedia systems

### Handheld Systems <a id="Handheld-Systems"></a>

* Personal Digital Assistants \(PDAs\)
* Cellular telephones
* HW specialized OS
* Issues
  * Limited memory
  * Slow processors
  * Battery consumption
  * Small display screens

#### Computer Systems <a id="Computer-Systems"></a>

![](https://i.imgur.com/tcuzlso.png)

