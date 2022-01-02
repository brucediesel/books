# The Linux Programming Interface
## Chapter 1

## Chapter 2
### 2.1 The Core Operating System: The Kernel
*Operating System* refers to the central software that managed and allocates the cokmputer resources (CPU, RAM, devices etc)
_Kernel_ is often used as a synonym.
The Linux Kernel typically resides at the pathname _/boot/vmlinuz_

**Tasks Performed by Kernel:**
- _Process Scheduling_ 
- _Memory Management_ Linux employs virtual memory management which has the following advantages:
  - Processes are isolated from each other
  - Only part of a process needs to be kept in memory thereby lowering the requirements of each process
- _A File System_ enabling files to be created, retrieved, updated and deleted
- _Process Creation/termination_ loading programs into memory, allocate resources, and this is termed a _process_.  Resources are freed when the process terminates.
- _Access to Devices_ peripherals attached to the computer allowing input, output etc  Kernel provides an interface that standardises and simplifies access to devices.
- _Networking_
- _System API_ enabling processes to request the kernel to perform various tasks using _system calls_.  the primary topic of this book.

**Kernel Mode and User Mode**
Modern processor architectures typically allow the cpu to operate in at least two different modes, _user mode_ and _kernel mode_. Correspondingly, areas of virtual memory can be marked as being part of **user space** or **kernel space**
When running in _user mode_ the CPU can only access virtual memory marked as being in user space.  In kernel mode, the CPU can access memory in both user and kernel memory space.

**Process versus kernel views of the system**
A _process_ operates in isolation, it does not know exactly where it is in memory, where files are located etc.  All functions it requires are mediated by the kernel.  A _kernel_ on the other hand, facilitates the running of all processes on the system.  It schedules the processes, and maintains data structures containing information about all running processes are created, change state, and terminate.

### 2.2 The Shell
_shell_ is a special purpose program designed to read commands typed by a user and execute appropriate programs in response to those commands.
_login shell_ is used to denote the process that is created to run a shell when the use first logs in.
In Unix systems, the shell is a _user process_.  There are many types of Unix shells:
- _Bourne shell (sh)_ is the oldest and widely used.  Included in all unix implementations.
- _C Shell (csh)_ resembles C language in amy flow control constructs.  Includes some useful features not in Bourne shell - command history, command-line editing, job control, aliases.
- _Korn Shell (ksh)_ successor to bash, is backward compatible with bash, but incorporates interactive features like csh.
- _Bourne Again Shell (bash)_ GNU's implementation of Bourne Shell, adds interactive features like in csh and ksh.  Most widely used on Linux.
