---
title: "1 Intro"
date: 2023-12-01T21:15:47+04:00
draft: true
---

**Resources** consist one or more processors, some main memory,
disks, printers, a keyboard, a mouse, a display, network interfaces, and various
other input/output devices.

The **Operating system**(**OS**) is a layer of software, whose job is to provide user programs with
a better, simpler, cleaner, model of the computer and to handle managing all the resources.

Pieses of the OS:

1. **kernel(supervisor) mode**
   In this mode it:
   - has complete access to hardware;
   - can execute any instruction the machine is capable of executing.
2. **user mode**
   It his mode it:
   - can execute only subset of all instructions.

Simple scheme.

| Modes  | Software               |
| ------ | ---------------------- | -------- |
| User   | Programs(e.g. browser) |
| User   | User interface program |
| Kernel | OS                     |
| -      | -                      | hardware |

In the user mode you can install different software, but in the kernel mode software depend from hardware and you can't install different software.

## 1 WHAT IS AN OPERATING SYSTEM?

The OS functions:

1. providing a clean abstract set of resources;
2. managing hardware resources.

**I/O** - input/output.

The **architecture** is instruction set, memory organization, I/O, and bus structure.

**Drivers** are software which interact with hardware(I/O devices) and provide an interface for simplifying.

Systems provide yet another layer of abstraction for using disks: files.

> The job of the operating system is to create good abstractions and then
> implement and manage the abstract objects thus created, because hardware is really ugly.

### 1.2 The Operating System as a Resource Manager

Ways of **multiplexing** (sharing) resources:

1. in **time**.
   Programs want to use same resource at the same time.
2. in **space**.
   Programs use same resource but every program can access only to own piece of resource.

## 2 HISTORY OF OPERATING SYSTEMS

The first true digital computer was designed by the English mathematician Charles Babbage (1792–1871).
Ada Lovelace was the first programmer.

### 2.1 The First Generation (1945–55): Vacuum Tubes

The World War II period stimulated an explosion of activity in computer developing.
Alan Turing.

Some were binary, some used vacuum tubes, some were programmable.
It took seconds to perform even the simplest calculation.

No programming languages and operation systems.

For programming was used plugboards and **punched cards**.

### 2.2 The Second Generation (1955–65): Transistors and Batch Systems

**Mainframes** were placed in computer rooms, with staff of proffesionals operators.
A **job** is a program or set of programs.

The first programming languages(FORTRAN and asm) and acestors of modern OS.

In the **batch system**, a tray full of jobs was collected in the input room and then was read onto a magnetic tape.

**Off line** means not connected to the main computer.

The structure of a typical input job:

```
$JOB -> $FORTRAN -> FORTRAN program -> $LOAD -> $RUN -> Data for program -> $END.
```

### 2.3 The Third Generation (1965–1980): ICs and Multiprogramming

> Yogi Berra reputedly said:
> "In theory, theory and practice are the same; in practice, they are not."

**ICs (Integrated Circuits)**.

The **OS/360** was a piece of assembly shit.

A **multiprogramming system** is a system, in which memory several jobs are placed
and if one job waits I/O, then another job work.

The **spooling (from Simultaneous Peripheral Operation On Line)** is the technique, which consists in loading
a new job from the disk into now-empty partition and runnit it after another job is finished.

The **timesharing** is a variant of multiprogramming, in which each user has an online terminal.
**CTSS (Compatible Time Sharing System)**.

**MULTICS (MULTiplexed Information and Computing Service)** was designed to support hundreds of users.

The **Cloud computing** is a concept, in which relatively small computers
are connected to servers in vast and distant data centers where all the computing is done,
with the local computer just handling the user interface.

Growth of minicomputers.

**MULTICS**  
||  
V  
**UNIX**  
||  
V

- **BSD**
- **System V**
- **MINIX** -> **Linux**

**POSIX** is the standard, that make a possible to write
programs that could run on any UNIX system.

### 2.4 The Fourth Generation (1980–Present): Personal Computers

**LSI (Large Scale Integration)** circuits—chips containing thousands
of transistors on a square centimeter of silicon.

The era of personal computers dawned.

**CP/M (Control Program for Microcomputers)** is a disk-based OS.

**DOS (Disk Operating System)**.

Bill Gates' company **Microsoft** + DOS developer -> **MS-DOS (MicroSoft Disk Operating System)**.

In the 1960s Doug Engelbart invented the **Graphical User Interface**,
complete with windows, icons, menus, and mouse.

Steve Jobs embarked on building an Apple with a GUI.

Apple Macintosh was **user friendly**, meaning that it was intended for
users who not only knew nothing about computers but furthermore
had absolutely no intention whatsoever of learning.

**Mac OS X** is a UNIX-based operating system.

**Windows 95** -> **Windows NT (where the NT stands for New Technology)** ->
**Windows Me (Millennium Edition)** -> **Windows XP**.

**Service packs** replaced variations of Windows OSs

**Windows Vista** had **Digital Rights Management**.

**Windows 7** was wide used, in contrast to it's successor **Windows 8** and ancestor **Windows Vista**.

**x86** processors based on the family of instruction-set architectures
that started with the 8086 in the 1970s.

**x86-32** and **x86-64** indicate 32-bit and 64-bit variants.

**FreeBSD**.

**X Window System (also known as X11)** is a windowing system for UNIX systems.

The growth of networks -> **network operating systems** and **distributed operating systems**.

Distributed OSs operates with multiple processor at time.

### 2.5 The Fifth Generation (1990–Present): Mobile Computers

40 kg phone.

The 1 kg brick.

Nokia combined **PDA (Personal Digital Assistant)** and phone in the N9000.

In 1997, Ericsson coined the term **smartphone**.

**Symbian**, **RIM** and **iPhone**.

**Android** is dominant due to open sources.

## 3 COMPUTER HARDWARE REVIEW

An operating system is intimately tied to the hardware of the computer it runs on.
It extends the computer’s instruction set and manages its resources.

### 3.1 Processors

The **CPU** is the 'brain' of the computer.

Cycle of every CPU:

1. fetch the first instruction from memory
2. decode it to determine its type and operands
3. execute it
4. fetch the next instruction from memory

Each CPU has a specific set of instructions that it can execute.

All CPUs contain some registers inside to hold key variables and temporary results.

Special registers:

- **program counter**

  contains the memory address of the next instruction to be fetched and
  automatically updates to the next instruction after fetching current instruction.

- **stack pointer**

  points to the top of the current stack in memory.

  The **stack** contains one frame for each procedure
  that has been entered but not yet exited.

  A procedure's stack frame holds those input parameters,
  local variables, and temporary variables that are not kept in registers.

- **PSW (Program Status Word)**

  contains the condition code bits, which are set by comparison instructions, the CPU
  priority, the mode (user or kernel), and various other control bits.

When time multiplexing the CPU, the operating system will often stop
the running program to (re)start another one.
Every time it stops a running program, the operating system
must save all the registers so they can be restored when the program runs later.

Many modern CPUs have facilities for executing more than one instruction at the same time:

- **pipeline**

  CPU might have separate fetch, decode, and execute units
  and all this units would be busy at the same time.

- **superscalar**

  CPU might have multiple execution units and
  instructions stored into holding buffer before executing if particular execution unit was busy.
  Instructions are often executed out of order and this foisted onto the operating system.

**system call** is a special kind of procedure call that has the additional property
of switching from user mode to kernel mode.

The `TRAP` instruction switches from user mode to kernel mode and starts the operating system.

**Moore's law**

> The number of transistors on a chip doubles every 18 months.

**multithreading** or **hyperthreading** allows the CPU to hold the state of
several different threads and then switch back and forth on a nanosecond time scale.

Each thread appears to the operating system as a _separate CPU_.

**Cores** are complete processors.

A **GPU(Graphics Processing Unit)** is a processor with, literally, thousands of tiny cores.

### 3.2 Memory

A typical memory hierarchy

| Access time | Name          | Capacity |
| ----------- | ------------- | -------- |
| 1nsec       | Registers     | <1 KB    |
| 2nsec       | Cache         | 4 MB     |
| 10nsec      | Main memory   | 1-8 GB   |
| 10msec      | Magnetic disk | 1-4 TB   |

The **registers** are placed inside the CPU and fast as the CPU.

Main memory is divided up into **cache lines**, typically 64 bytes.
The most heavily used cache lines are kept in a high-speed cache.
A **cache hit** happens when the program needs to read a memory word and
the cache hardware found the needed line in the cache.

In any caching system, several questions come up fairly soon, including:

1. When to put a new item into the cache.
2. Which cache line to put the new item in.
3. Which item to remove from the cache when a slot is needed.
4. Where to put a newly evicted item in the larger memory.

(16 KB)**L1 cache** is the first level of cache that is placed inside the CPU and
usually feeds decoded instructions into the CPU's execution engine.

(several megabytes)**L2 cache** is the second level of cache that holds several megabytes of
recently used memory words.

The difference between the L1 and L2 caches lies in the timing.

Main memory is usually called **RAM (Random Access Memory)** or **core memory**, that is volatile.
All CPU requests that cannot be satisfied out of the cache go to main memory.

**ROM (Read Only Memory)** is a non-volatile random-access memory,
that is programmedat the factory and cannot be changed afterward.

**EEPROM (Electrically Erasable PROM)** and **flash memory** are also non-
volatile, but in contrast to ROM can be erased and rewritten.

**Flash memory** wears out, if it is erased too many times.

**CMOS memory** is the volatile memory which many computers use to hold the current time and date.

### 3.2 Disks

**hard disk** is a mechanical device that is used to store big quantities of data.

A disk consists of one or more metal rotating **platters**.
Information is written onto the disk in a series of concentric circles.

At any given arm position, each of the heads can read an annular region called a **track**.
Together, all the tracks for a given arm position form a **cylinder**.
Each track is divided into some number of **sectors**.

**SSDs (Solid State Disk)** store a lot of data in (Flash) memory.

**Virtual memory** makes it possible to run programs larger than physical memory
by placing them on the disk and using main memory
as a kind of cache for the most heavily executed parts.

**MMU (Memory Management Unit)** converts the addresses generated by the program
to the physical address in RAM where the word is located.

A **context switch** is a switching from one program to another.

### 3.4 I/O Devices

I/O devices consist of two parts:

- **controller**

  is a chip or a set of chips that physically controls the device.

- device itself

Devices have fairly simple interfaces, both because
they cannot do much and to make them standard.

A **device driver** is the software that talks to a controller,
giving it commands and accepting responses.

3 ways the driver can be put into the kernel:

1. to relink the kernel with the new driver and then reboot the system

2. to make an entry in an operating system file telling it that
   it needs the driver and then reboot the system

3. to accept new drivers while running and install them on the fly without the need to reboot

The **I/O port space** is the collection of all the device registers.

2 ways of modifying devices registers:

1. the device registers are mapped into the operating system's address space

2. special IN and OUT instructions are available in kernel mode
   to allow drivers to read and write the registers

3 ways of input and output:

1. **busy waiting**

   A user program ->_(system call)_ the kernel ->_(procedure call)_ -> the appropriate driver.

   The driver waits until the device isn't done and returns the data.

   The operating system then returns control to the caller.

   \- the _disadvantage_ of tying up the CPU polling the device until it is finished.

2. **interrupt**

   The driver tells the controller what to do by writing into its device registers.
   The controller then starts the device.

   The controller signals the interrupt controller chip using certain bus lines, when it has finished.

   If the interrupt controller is ready to accept the interrupt,
   it asserts a pin on the CPU chip telling it.

   The interrupt controller puts the number of the device on the bus.

   The CPU switches into kernel mode and starts appropriate inetrrupt handler,
   which the CPU determines due to interrupt vector.

   The **interrupt vector** is memory in which the device number may be used as an index to
   find the address of the interrupt handler for this device.

   When the **interrupt handler** is all finished,
   it returns to the previously running user program
   to the first instruction that was not yet executed.

3. **DMA (Direct Memory Access) chip**

   can control the flow of bits between memory and
   some controller without constant CPU intervention.
   The DMA chip causes interrupt if it is finished.

### 3.5 Buses

**Buses** are used to transfer data between components of a system.

The main bus is the **PCIe (Peripheral Component Interconnect Express)** bus.

A **shared bus architecture** means that multiple devices use the same wires to transfer data.

A **parallel bus architecture** means that you send each word of data over multiple wires.

A **serial bus architecture** means that all bits will be sent in message through a single connection, known as a **lane**.

Buses:
**DMI (Direct Media Interface)**,
**USB (Universal Serial Bus)**,
**SCSI (Small Computer System Interface)**.

In the **plug and play** PC system design, the system automatically collect information
about the I/O devices, centrally assign interrupt levels and I/O addresses,
and then tell each card what its numbers are.

### 3.6 Booting the Computer

**BIOS (Basic Input Output System)** contains low-level I/O software.
It is held in a flash RAM, which can be updated by the OS.

1. Scanning the PCIe and PCI buses

2. Configuring new devices

3. Examining the partition table at the end of the boot
   sector to determine which partition is active

4. The loader reads in the operating system
   from the active partition and starts it

5. The OS queries the BIOS to get the configuration information

6. The OS start if drivers are presented for all devices

## 4 THE OPERATING SYSTEM ZOO

### 4.1 Mainframe Operating Systems

- prodigious amounts of I/O
- gigantic I/O capacity

3 kinds of services:

- **batch**

  processes routine jobs without any interactive user present

- **transaction processing**

  handle large numbers of small requests

- **timesharing**

allow multiple remote users to run jobs on the computer at once

### 4.2 Server Operating Systems

- multiple users at once over a network

- allows the users to share resources

### 4.3 Multiprocessor Operating Systems

Multiple CPUs are connected into a single system to
get major-league computing power.

And specific OSs are required to deal with this CPUs.

### 4.4 Personal Computer Operating Systems

- good support to a single user

### 4.5 Handheld Computer Operating Systems

**PDA (Personal Digital Assistant)**

- a lot of sensors

- third-party applications (**apps**)

### 4.6 Embedded Operating Systems

- run on the computers that control devices

- do not accept user-installed software

- all the software is in ROM

### 4.7 Sensor-Node Operating Systems

- networks of tiny sensor nodes

- each sensor node is a real computer

- small and simple

### 4.8 Real-Time Operating Systems

- the action absolutely must occur at a certain moment

1. **hard real-time system**

2. **soft real-time system**

### 4.9 Smart Card Operating Systems

- the smallest operating systems

- some are Java oriented

- severe power limits

## 5 OPERATING SYSTEM CONCEPTS

### 5.1 Processes

A **process** is basically a program in execution.
Processes have two parts:

1. address space,
2. process table entry.

A **address space** a associated with process list of memory locations
from 0 to some maximum, which the process can read and write.

Periodically, the operating system decides to stop running
one process and start running another.
The first process is **suspend**.

When a process is temporarily suspended,
all information about process must be saved.

The **process table** is a system table (an array of structures)
that consists all information about each process.

The **core image** is process's address space.

**Child processes** are processes that were created by another process.

**interprocess communication**.

**Signals** are the software analog of hardware interrupts.
When a process receive a signal, then special signal-handling procedure is started.

Each user in the system has **UID (User IDentification)** and **GID (Group IDentification)**.
Every process started has the UID of the person who started it.

The **superuser** , or **Administrator**, is a user with special powers.

### 5.2 Address Spaces

The OS need to keep processes from writing to address spaces of another processes.

Computers addresses are 32 or 64 bits.

**Virtual memory** is a technique in which the OS
keeps part of the address space in main memory and part on
disk and shuttles pieces back and forth between them as needed.

Address space is decoupled from computer main memory.

### 5.3 Files

**Files** are abstract model.

A **directory** is a way of grouping files together.

The file hierarchies are organized as tree.

The **root directory** is the top of the directory hierarchy.

Every file can be specified by its **path name**.

Each process has a current **working directory**.

Path names can be

- **absolute**(unique path from root directory) and
- **relative**(path from working directory).

A **file descriptor** is a integer associated with file.

**root file system**.

To access files from some external media you have to **mount**
device file system on a directory of the root file system
(have to be attached to the root file system).

**Special files** are provided in order to make I/O devices look like files.

**Block special files** are used to model devices that
consist of a collection of randomly addressable blocks.

**Character special files** are used to model devices that accept or output a character stream.

A **pipe** is a sort of pseudofile that can be used to connect two processes.

### 5.4 Input/Output

Every OS has an I/O subsystem for managing its I/O devices.

### 5.5 Protection

The OS manage the security of files
(In UNIX OS a 9-bit binary(**rwx**) protection code is assigned to each file).

## 6 SYSTEM CALLS

A system call mechanism are often implemented in assembly code.

Steps to perform a system call:

1. Push parameters onto the stack.
2. Call the library procedure.
3. Put the system-call number in a register.
4. Trap to the kernel (switch to kernel mode and jump to the specific allowed address).
5. Execution at a fixed address within the kernel.
6. Find needed handler in a table of pointers to system-call handlers indexed on system-call number.
7. The system-call handler runs.
8. Trap to the user-space and return to the library procedure.
9. Return to the user program.
10. Increment $SP$ to clear the stack.

### 6.1 System Calls for Process Management

**PID** stands for Process IDentifier.

Processes in UNIX have their memory divided up into three segments:

1. the **text segment** (the program code)

2. the **data segment** (the variables)

   - grows upward
   - expansion should be done with special system call(`brk()`)

3. the **stack segment**

   - grows downward
   - expansion happens automatically

### 6.2 System Calls for File Management

Associated with each file is a pointer that indicates the current position in the file.

The `lseek()` call changes the value of the position pointer.

### 6.3 System Calls for Directory Management

Every file in UNIX has a unique number, its **i-number**, that identifies it.

A i-number is an index into a table of **i-nodes**, one per file,
telling who owns the file, where its disk blocks are, and so on.

**Partitions or minor devices** are portions of hard disks.

### 6.4 Miscellaneous System Calls

In the year 2106, `time()` value will be overflowed.

### 6.5 The Windows Win32 API

A set of procedures called the **Win32 API (Application Programming Interface)**.

## 7 OPERATING SYSTEM STRUCTURE

### 7.1 Monolithic Systems

- The all OS is a large executable.

- Structure of levels of procedures.

- loadable extensions are **DLLs (Dynamic-Link Libraries)** or **shared libraries**.

### 7.2 Layered Systems

- Each layer setups new abstractions and allows layers
  above use this abstractions with system calls.

### 7.3 Microkernels

- The OS is set of modules.

- Each module are independent.

- There are several layers of modules.

- **reincarnation server**.

- **Mechanism** are placed in the kernel but not the **policy**.

### 7.4 Client-Server Model

- Two classes of processes: the **servers** and the **clients**.

- Servers and clients can be different machines.

### 7.5 Virtual Machines

1. **type 1 hypervisor**

   - Manages all resources by itself.

2. **type 2 hypervisor**

   - Uses a host operating system

- Allows install several independent OSs on the one machine.

- **binary translation**.

- The guest OS does the same thing it does on the actual hardware.

### 7.6 Exokernels

- Each virtual machine can use only resources assigned to it.

- Each user-level virtual machine can run its own OS.
