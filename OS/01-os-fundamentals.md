# 1. OS Fundamentals

## 1. What Is an Operating System? ⭐⭐⭐

An **Operating System (OS)** is system software that manages hardware and provides useful, safe abstractions to programs and users.

```text
Applications
     ↓
Operating System
     ↓
CPU | Memory | Disk | Network | Devices
```

Without an OS, every program would need to control hardware directly.

### Goals

```text
Convenience → make hardware practical to use
Efficiency  → use CPU, memory, and devices well
Protection  → isolate users/processes and control access
```

## 2. Main Responsibilities

```text
Process management  → run and schedule programs
Memory management   → allocate/protect RAM
File management     → organize persistent data
Device / I-O        → use keyboard, disk, network, etc.
Protection          → isolate users and programs
```

The OS is both a **resource manager** and a **control program**.

### Core Components

```text
Kernel / scheduler     → CPU and process control
Memory manager         → address spaces and RAM allocation
File system            → persistent files/directories
I/O subsystem + drivers→ device access
Security/protection    → permissions and isolation
```

## 3. Kernel vs User Space ⭐⭐⭐

The **kernel** is the privileged core of the OS. Applications run in **user space** with restricted privileges.

```text
User space    → browser, editor, app code
     ↓ system call
Kernel space  → scheduler, memory manager, drivers
     ↓
Hardware
```

Why separate them? A faulty app should not be able to overwrite disk sectors or read another process's memory.

### User mode vs kernel mode

```text
User mode   → restricted; normal application code
Kernel mode → privileged; can execute sensitive instructions
```

An interrupt, exception, or system call transfers control to the kernel; returning resumes user mode.

## 4. System Calls ⭐⭐⭐

A **system call** is the controlled interface through which a program asks the kernel for a service.

Examples:

```text
Process  → create process, exit, wait
File     → open, read, write, close
Memory   → allocate/map memory
Network  → create socket, send, receive
```

```text
App calls read()
      ↓
System-call boundary
      ↓
Kernel validates request and accesses device
      ↓
Result returned to app
```

> A library function is not necessarily a system call; it may do work entirely in user space.

## 5. OS Structures

| Structure | Core idea | Example idea |
|---|---|---|
| Monolithic kernel | Most OS services run in kernel space | Fast, but large trusted kernel |
| Microkernel | Keep only essentials in kernel; services in user space | Better isolation, IPC overhead |
| Hybrid kernel | Mixes monolithic performance with microkernel-style ideas | Practical compromise in many systems |
| Layered | Each layer uses lower layers | Easier reasoning |
| Modular | Loadable kernel components | Practical modern approach |

Most modern systems combine ideas rather than fitting one pure model.

## 6. Types of Operating Systems

```text
Batch          → jobs collected and run with little interaction
Time-sharing   → CPU rapidly switches among interactive users/tasks
Real-time      → deadlines matter; correctness includes timing
Distributed    → multiple machines cooperate
Embedded/mobile→ purpose-specific, resource constrained
```

**Hard real-time:** missing a deadline can be unacceptable.  
**Soft real-time:** occasional misses reduce quality but may be tolerated.

## 7. Boot Process

```text
Power on
  ↓
Firmware (BIOS/UEFI)
  ↓
Bootloader
  ↓
Kernel loaded into memory
  ↓
Kernel initializes devices and starts first user-space process
  ↓
Services / login / desktop
```

## 8. Interrupts, Traps and Exceptions

| Event | Meaning | Example |
|---|---|---|
| Interrupt | Usually external, asynchronous event | keyboard input, disk completion |
| Trap / system-call trap | Intentional transfer to kernel | `read()` |
| Exception | Synchronous CPU-detected event | divide by zero, page fault |

## 🧠 One-Minute Revision

```text
OS → manages hardware and provides abstractions
Kernel → privileged OS core
User space → applications, restricted privileges
System call → safe request from app to kernel
Interrupt → external event
Exception → synchronous CPU event
```

> **Kernel mode is about privilege, not simply “the OS is running.”**
