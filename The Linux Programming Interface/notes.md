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

Shells not only interactive, but also interpret _shell scripts_ which are text files containing shell commands - as such, each shell has facilities resembling programming languages: variables, loop and conditional statements, I/O commands, and functions.

### 2.3 Users and Groups
Each _user_ on the system is uniquely identified.  Users may beling to _Groups_.
**Users**
Every user has a unique _login Name_ (username) and a corresponding numeric _user id_ UID, defined by a line in the system _password file_ **/etc/passwd** including the following information:
- _Group ID_ the numeric group id of the first of the groups the user belongs to.
- _Home Directory:_ the initial directory into which the user is placed after logging in.
- _Login Shell_ the name of the shell for that user.

The password record may also include the users password, in encrypted form, but often stored in a separate _shadow password file_ readable only by priveleged users.
**Groups**
Enables users to be placed in _groups_ to receive privileges enabled by that group.  Each group identified by a single line in the _system group file_ **/etc/group** which contains:
- _Group Name_
- _Group ID_
- _User List_ comma separated list of login names of members of that group.

**Superuser**
The _SuperUser_ (ID = 0, username = root) has special privileges on the system.  _root_ bypasses all permission checks on the system, can access any file, or send signals to any process in the system.  Used to perform administrative tasks on the system.

## 2.4 Single Directory Hierarchy, Directories, Links, and Files
The kernel maintains a single hierarchical directory structure to organize all files in the system.  At the base of this hierarchy is the _root directory_ named **/** (slash).  All files and directories are children of the root directory. (Windows has each disk device with it's own directory hierarchy.)
![image](https://user-images.githubusercontent.com/7336290/147913088-5e78bb6e-d925-4b71-9999-3d01e7e07b22.png)
**File Types**
Within the file system, each file is marked with a _type_.  One file type denotes ordinary data files, usually called _regular_ or _plain_ files.  Other file types include _devices, pipes, sockets, directories, and symbolic links._  The term _file_ is used to denote a file of any type, not just a regular file.
**Directories and Links**
A _directory_ is a special file whose contents are a table of filenames coupled with references to the corresponding files.  This _filename-plus-reference_ is called a _**link**_.  Files can have multiple links, and thus multiple names, in the same or different directories.
![image](https://user-images.githubusercontent.com/7336290/147918319-18cb56d4-f0e9-428c-931c-b9fe39e95def.png)
Every directory contains at least two entries - **.** (dot - points to itself) and **..** (dot-dot - points to it's parent).
**Symbolic Links**
A _symbolic link_ provides an alternate name for a file (or directory).  It is just a specially marked file containing the name of another file (called the _target_ file).
If a symbolic link refers to a file that does not exist, it is a _dangling link_.

