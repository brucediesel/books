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

A _directory_ is a special file whose contents are a table of filenames coupled with references to the corresponding files.  This _filename-plus-reference_ is called a _**link**_.  Files can have multiple links, and thus multiple names, in the same or different directories.  Directories can contain links to both files and other directories.

![image](https://user-images.githubusercontent.com/7336290/147918319-18cb56d4-f0e9-428c-931c-b9fe39e95def.png)

Every directory contains at least two entries - **.** (dot - points to itself) and **..** (dot-dot - points to it's parent).

**Symbolic Links**

A _symbolic link_ provides an alternate name for a file (or directory).  It is just a specially marked file containing the name of another file (called the _target_ file).
If a symbolic link refers to a file that does not exist, it is a _dangling link_.
Often _hard link_ and _soft link_ are used as alternate terms for normal and symbolic links.

**Filenames**

Typically filenames can be up to 255 charactgers long.  They may contain any characters except slashes (/) and null characters (\0).  It is adviseable to only use characters from the _protable filename character set_.  [-.\_a-zA-Z0-9] to avoid filename characters that have special meanings in the shell, regular expressions etc.  If special characters have been used, they have to be _escaped_ (\).  Filenames should also not start with a -, to avoid being interpreted as an option in a shell command.

**Pathnames**

A _pathname_ is a string consisting of an optional initial slash (/) followed by a series of filenames separated by slashes.  All but the last of these component filenames identifies a directory (or a symbolic link that resolves to a directory).  The last component of a pathname may identify any type of file, including a directory.  The string .. can also be used anywhere in a pathname to refer to the parent of the location so far specified in the pathname.
A pathname desrcibes the location of a file within the single directory hierarchy and is either absolute or relative:
- An _absolute_ pathname begins with a slash (/) and specifies the location of a file with respect to the root directory.
- A _relative_ pathname specifies the cloation of a file relative to a process' current working directory and is distinguished from an absolute pathname by the absence of the initial slash.

**Current Working Directory**

Each process has a _current working directory_.  This is the process' current location within the single directory hierarchy.  A process inherits it's current working directory from it's parent process.  A login shell has its initial current working directory set to the location named in the home directory field of the user's password file entry.

**File Ownership and Permissions**

Each file has an associated user ID and group ID that define the owner of the file and the group to which it belongs.  For the purposes of accsssing a file the system divides users into three categories:
1. The _owner_ of the file.
2. Users who are members of teh group matching he file's _group_ ID
3. The rest of the world _other_.
Permission bits are set for each of these categories - _read_ permissions, _write_ permissions, _execute_ permissions.
Permissions may also be set on directories but have slightly different meaning:
- _read_ allows the contents of directory to be listed,
- _write_ allows contents of directory to be changed (filenames can be added, removed, changed)
- _execute_ (sometimes called _search_) allows access to files within directory (subject to file permissions)

## 2.5 File I/O Model

Unix implements a concept of _universality of I/O_ which means that the same system calls (_open(), read(), write(), close() etc_) are used to perform I/O on all types of files, includiong devices.  Thus, programs using these calls will work with any type of file.
The kernel exxentially provides one file type: a sequential stream of bytes, which, in the case of disk files, disks, and tape devices, can be randomly accessed using the _lseek()_ system call.
Many application and libraries interpret the _newline_ character as terminating one line of text and commencin another.  UNIX systems have no _end-of-file_ character.  End of file is interpreted when _read()_ returns no data.

**File Descriptors**

The I/O system calls refer to open files using a _file descriptor_ which is usually an integer.  The file descriptor is obtained using a call to _open()_ which takes a pathname, and returns a file descriptor if file is successfully opened.
Normally, a process inherits three open file descriptors when it is started by the shell.  **descriptor 0** is _standard input_, the file from which the process takes it's input; **descriptor 1** is _standard output_, the file to which the process writes it's output; and **descriptor 3** is _standard error_, the file to which the process writes error messages.  In an interactive shell, these three descriptors are normally connected to the terminal.  In the _stdio_ library, these descriptors correspond to file streams _stdin_, _stdout_, and _stderr_.

**The _stdio_ Library**

To perform file I/O, C programs typically employ I/O funtions contained in the standard C library (_fopen()_, _fclose()_, _scanf()_ etc) which are layered on top of I/O system calls (_open()_, _close()_, _read()_ etc)

## 2.6 Programs

_Programs_ normally exist in two forms, _source code_ which is human readable text consisting of a series of statements written in a programming language, or _binary machine language_ which is source code compiled into instructions the computer can understand.  A _script_ is a text file containing commands that are directly processed by a shell or command interpreter (e.g. bash script, python program etc)

**Filters**

A _filter_ is the name often applied toa program that reads its input from _stdin_, performs some transformation of that input, and writes the transformed data to _stdout_.

**Command line arguments**

Programs can access the _command line arguments_, the words that are supplied to the command line when the program is run.
```C
int main(int argc, char* argv[])
```

## 2.7 Processes

A _process_ is an instance of an executing program.  From a kernel point of view, processes are entities among which the kernel must share the various resources of the computer.

**Process Memory Layout**

A process is logically divided into the following parts, known as _segments_:
- _text_ which are the intstructions of the program
- _data_ which are the static variables used by the program
- _heap_ an area where programs can dynamically allocate memory
- _stack_ an area of memory that grows and shrinks as functions are called and return - used to allocate storage for local variables and function call linkage information
 
![image](https://user-images.githubusercontent.com/7336290/147929160-4aec9ede-226c-4405-b9b0-b7d44d037f6e.png)

**Process Creation and program execution**

A process can create a new process using the _fork()_ system call.  The process that calls fork is the _parent_ and the new process is the _child_ process.  The kernel creates the child process by making a duplicate of teh parent process, which inherits copies of the parent's data, stack, and heap segments.  Which is may then modify independently of the parent's copies.  The child process frequently uses the _execve()_ system call to load and execute an entirely new program.  An execve() call destroys existing text, data, stack and heap segments, replacing them with new segments based on the code of the new program.

**Process ID and Parent Process ID**

Each process has a unique integer _process idenitifer (PID)_.  Each process also has a _parent process id (PPID)_ which identifies the process that requested the kernel to create it.

**Process Termination and termination status**

A process can terminate in one of two ways: by requesting it's own termination using the _\_exit()_ system call (or related _exit()_ library function) or by being killed by the delivery of a signal.  In either case the process yeilds a _termination status_ which is a small non-negative integer value that is available for inspection by the parent process using the _wait()_ system call.  If _exit()_ is used, the process explicitly sets it's own termination code.  If terminated by a signal, the termination status is set according to the type of signal that caused the termination.  By convention, the termination status of 0 indicates that the process succeeded, and a non-zero status indicates that an error occurred.  Most shells make the termination status available via a shell variable named **_$?_**.

**Process user and group identifiers (credentials)**

Each process has a number of associated user IDs (UIDs) and group IDs (GIDs).  These include:
- _real user ID_ and _real group ID_ identifying the user and group to which the process belongs.  New processes inherit these IDs from their parents.
- _effective user id_ and _effective group id_ used to determine the permissions that the process has when accessing protected resources such as files and interprocess communication objects.  Often have same values as corresponding real IDs.  Effective IDs enable processes to assume privileges from another user or group.
- _Supplementary grou IDs_ identify additional groups to which the process belongs.  Login shells get these form the /etc/group file.

**Privileged processes**

A _privileged_ process is one whose _effective_) user ID is 0 (superuser).  This process bypasses the permission restrictions normally applied by the kernel.
A process may be privileged because it was created by another privileged process e.g. login shell started by _root_, or by _set user ID_ mechanism.

**Capabilities**

Linux divides privileges traditionally accorded to the superuser into a set of distinct units called _capabilities_.  Each privileged operation is associated with a particular capability, and a process can only perform an operation if it has the corresponding capability.  Capabilities names begin with the prefix __CAP\___. e.g. __CAP\_KILL__

**The _init_ Process**

When booting the system, the kernel creates a special process call _init_, the "parent of all processes" which is derived from the program file `/sbin/init`.  All processes on the system are created (using _fork()_) either by _init_ or by one of it's descendents.  The _init_ process always has the process ID 1 and runs with superuser privileges.  This process can't be killed, even by superuser and terminates with the system is shut down.

**Deamon Processes**

A _deamon_ is a special-purpose process that is created and handled by the system in the same way as other processes, but which is distinuished by the following:
- it is long lived, often starting at system boot, and remains until system shutdown
- runs in the background, and has no controlling terminal from which is can read input and write output.
e.g. _syslogd_

**Environment List**

Each process has an _environment list_, whic is set of _environment variables_ that are maintained within the user-space memory of the process.  Each element is a name-value pair which are inherited from parent when process is created.  This enables a parent to communicate to child process.  When a program replaces the program that it is running using _exec()_, the new program either inherits the environment used by the old program, or receives a new environment specified as part of the _exec()_ call.
Environment variables are create with the **_export_** command in most shells e.g.

`$ export MYVAR='Hello World'`

C programs can access teh environment using the external variable _(char \**environ)_ with libray functions that enable a process to retrieve and update values in it's environment.

**Resource Limits**

Each process consumes resources, such as open files, memory, CPU etc.  Using the _setrlimit()_ system call, a process can establish upper limits on it's consumption.  Each such _resource limit_ has two associated values: a _soft_ limit, which limits the amount of the resource that the process may consume and a _hard_ limit whic is the ceiling on the value to which the soft limit can be adjusted.
When a process is created with _fork()_, it inherits copies of its parent's resource limit settings.
The resource limits of a ahell can be adjusted using the **_ulimit_** command which are passed on to the child processes the shell creates.
