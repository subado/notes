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
$JOB -> $FORTRAN -> FORTRAN program -> $LOAD -> $RUN -> Data for program -> $END.

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
