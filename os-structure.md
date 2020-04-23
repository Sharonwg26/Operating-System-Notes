# Ch2 OS Structure

**tags: Operating System**

## OS Services <a id="OS-Services"></a>

### User interface \(介面\) <a id="User-interface-&#x4ECB;&#x9762;"></a>

#### CLI \(Command Line Interface\) \(命令行界面\) <a id="CLI-Command-Line-Interface-&#x547D;&#x4EE4;&#x884C;&#x754C;&#x9762;"></a>

* Fetches a command from user and executes it
* Shell: Command-line interpreter \(CSHELL, BASH\)

#### GUI \(Graphic User Interface\) \(圖形用戶界面\) <a id="GUI-Graphic-User-Interface-&#x5716;&#x5F62;&#x7528;&#x6236;&#x754C;&#x9762;"></a>

* Usually mouse, keyboard, and monitor
* **Icons**\(圖標\) represent files, programs, actions, etc

#### Most systems have both CLI and GUI <a id="Most-systems-have-both-CLI-and-GUI"></a>

### Communication \(通訊\) <a id="Communication-&#x901A;&#x8A0A;"></a>

Communication may take place using either **message passing or shared memory**

![ Msg Passing                                                                                 Shared Memory](https://i.imgur.com/KTXIxoc.png)

## Applications-OS Interface <a id="Applications-OS-Interface"></a>

### System calls <a id="System-calls"></a>

#### Request OS services <a id="Request-OS-services"></a>

* **Process control**  abort, create, terminate\(中止\) process allocate/free memory
* **File management**  create, delete, open, close file
* **Device management**  read, write, reposition device
* **Information maintenance**  get time or date
* **Communications**  send receive message
* The OS interface to a running program
* An explicit request to the kernel made via a software interrupt
* Generally available as assembly-language instructions

#### System Calls: Passing Parameters <a id="System-Calls-Passing-Parameters"></a>

* **Three general methods**.
  * Pass parameters in registers
  * Store the parameters in a table in memory, and the table address is passed as a parameter in a register  
  * Push \(store\) the parameters onto the stack by the program, and pop off the stack by operating system

![](https://i.imgur.com/VvlyKcU.png)

#### EXAMPLES OF WINDOWS AND UNIX SYSTEM CALLS <a id="EXAMPLES-OF-WINDOWS-AND-UNIX-SYSTEM-CALLS"></a>

The following illustrates various equivalent system calls for Windows and UNIX operating systems.

|  | Windows | Unix |
| :--- | :--- | :--- |
| Process control | CreateProcess\(\) | fork\(\) |
|  | ExitProcess\(\) | exit\(\) |
|  | WaitForSingleObject\(\) | wait\(\) |
| File management | CreateFile\(\) | open\(\) |
|  | ReadFile\(\) | read\(\) |
|  | WriteFile\(\) | write\(\) |
|  | CloseHandle\(\) | close\(\) |
| Device management | SetConsoleMode\(\) | ioctl\(\) |
|  | ReadConsole\(\) | read\(\) |
|  | WriteConsole\(\) | write\(\) |
| Information maintenance | GetCurrentProcessID\(\) | getpid\(\) |
|  | SetTimer\(\) | alarm\(\) |
|  | Sleep\(\) | sleep\(\) |
| Communications | CreatePipe\(\) | pipe\(\) |
|  | CreateFileMapping\(\) | shm open\(\) |
|  | MapViewOfFile\(\) | mmap\(\) |
| Protection | SetFileSecurity\(\) | chmod\(\) |
|  | InitlializeSecurityDescriptor\(\) | umask\(\) |
|  | SetSecurityDescriptorGroup\(\) | chown\(\) |

### API \(Application Program Interface\) <a id="API-Application-Program-Interface"></a>

* Users mostly program against API instead of system call  

![](https://i.imgur.com/OAApz0H.png)

#### Three most common APIs <a id="Three-most-common-APIs"></a>

* **Win32 API for Windows**
* **Java API** for the Java virtual machine \(JVM\)
* **POSIX\(Portable Operating System Interface for Unix\) API** for POSIX-based systems

**API – System Call – OS Relationship**

![](https://i.imgur.com/pnvctfe.png)

* Standard C Library Example  C program invoking printf\(\) library call, which calls write\(\) system call  

![](https://i.imgur.com/WFTZoOz.png)

#### Advantage <a id="Advantage"></a>

* Simplicity
* Portability
* Efficiency

## OS Structure <a id="OS-Structure"></a>

* User goals  operating system should be easy to use and learn, as well asreliable, safe, and fast
* System goals  operating system should be easy to design, implement, and maintain, as well as reliable, error-free, and efficient

### Simple OS Architecture <a id="Simple-OS-Architecture"></a>

### Layer OS Architecture <a id="Layer-OS-Architecture"></a>

### Microkernel OS <a id="Microkernel-OS"></a>

* Moves as much from the kernel into **user space**
* Communication is provided by **message passing**
* Easier for extending and porting  

![](https://i.imgur.com/HcG5F1E.png)

### Modular OS Structure \(模組化\) <a id="Modular-OS-Structure-&#x6A21;&#x7D44;&#x5316;"></a>

### Virtual Machine <a id="Virtual-Machine"></a>

* A virtual machine takes the layered approach to its logical conclusion
* A virtual machine provides an interface identical to the underlying bare hardware
* Difficult to achieve due to **“critical instruction”**  

![](https://i.imgur.com/8SKpRMG.png)

**Vmware \(Full Virtualization\)**

Run in user mode as an application on top of OS  
 

![](https://i.imgur.com/zaHd6Wv.png)

#### Para-virtualization: Xen <a id="Para-virtualization-Xen"></a>

* Presents guest with system similar but not identical\(相同\) to the guest’s preferred systems \(Guest must be modified\)
* Hardwar rather than OS and its devices are virtualized
* Within a container \(zone\) processes thought they are the only processes on the system  

![](https://i.imgur.com/H5uVGfp.png)

### Java Virtual Machine <a id="Java-Virtual-Machine"></a>

* Compiled Java programs are platform-neutral bytecodes executed by a Java Virtual Machine \(JVM\)
* consists
  * class loader
  * class verifier
  * runtime interpreter
* **Just-In-Time \(JIT\)** compilers increase performance  

![](https://i.imgur.com/06jsX2L.png)

## 補充 <a id="&#x88DC;&#x5145;"></a>

### OS/2 <a id="OS2"></a>

![](https://i.imgur.com/3IDWOIX.png)

### Mac OS X Structure hybrid structured <a id="Mac-OS-X-Structure-hybrid-structured"></a>

![](https://i.imgur.com/oy3Yxxk.png)

## 未上 <a id="&#x672A;&#x4E0A;"></a>

### Program Execution <a id="Program-Execution"></a>

### I/O operations <a id="IO-operations"></a>

### File-system manipulation <a id="File-system-manipulation"></a>

### Error detection \(檢測\) <a id="Error-detection-&#x6AA2;&#x6E2C;"></a>

### Resource allocation <a id="Resource-allocation"></a>

### Accounting <a id="Accounting"></a>

### Protection and security <a id="Protection-and-security"></a>

