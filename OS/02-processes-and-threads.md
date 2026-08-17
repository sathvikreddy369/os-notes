# 2. Processes and Threads

## 1. Program vs Process ⭐⭐⭐

```text
Program → passive file/instructions on disk
Process → program currently executing, with state and resources
```

Opening the same program twice can create separate processes.

## 2. Process Components

```text
Process address space
├── Code / text       → instructions
├── Data              → global/static variables
├── Heap              → dynamic allocation
└── Stack             → function calls, local variables
```

The OS stores process metadata in a **PCB (Process Control Block)**:

```text
PID | process state | program counter | registers
scheduling data | memory info | open files | accounting info
```

## 3. Process States ⭐⭐⭐

```text
New → Ready → Running → Terminated
              │  ↑
              ↓  │ preemption
           Waiting
          (I/O/event)
```

```text
Ready   → can run, waiting only for CPU
Running → currently executing on a CPU
Waiting → cannot run until an event occurs (e.g., I/O completion)
```

> **Ready ≠ waiting.** A ready process is runnable now.

## 4. Context Switch

A **context switch** saves the current process/thread state and restores another's state.

```text
Process A registers/PC → PCB
                         ↓
                     Scheduler
                         ↓
PCB → registers/PC for Process B
```

It enables multitasking but is **overhead**: no useful application work happens during the switch. Switching between processes generally costs more than between threads because address-space state may change.

## 5. Process Creation and Termination

A parent can create a child process. The child gets a new PID and may inherit selected resources.

```text
Parent
  └── creates → Child
```

On Unix-like systems, `fork()` conceptually creates a child that continues from the same point as its parent; `exec()` replaces that process's program image. `wait()` lets a parent collect a terminated child's status. Modern systems commonly use **copy-on-write** so memory is not fully copied immediately.

**Zombie:** child has exited, but parent has not collected its exit status.  
**Orphan:** parent exits before child; OS/adopter process takes responsibility.

## 6. Threads ⭐⭐⭐

A **thread** is the smallest unit of CPU execution inside a process.

```text
One process
├── Thread 1 → registers, PC, stack
├── Thread 2 → registers, PC, stack
└── Shared   → code, heap, open files, address space
```

### Process vs Thread

| Process | Thread |
|---|---|
| Own address space/resources | Shares process resources |
| Stronger isolation | Easy data sharing |
| Creation/switch usually heavier | Creation/switch usually lighter |
| Crash is usually isolated | A bad thread can crash its whole process |

## 7. Why Multithreading?

```text
Responsiveness → UI stays usable while work runs
Parallelism    → multiple CPU cores can work simultaneously
Resource share → threads naturally share process data
```

But shared memory introduces race conditions and synchronization needs.

### Concurrency vs Parallelism

```text
Concurrency → multiple tasks make progress during overlapping time periods
Parallelism  → multiple tasks execute at the same instant on multiple CPU cores
```

One CPU can provide concurrency by switching rapidly. Parallelism requires more than one execution resource.

## 8. User-Level vs Kernel-Level Threads

```text
User-level threads   → runtime/library schedules them; kernel may not see each one
Kernel-level threads → kernel schedules each thread
```

Kernel threads can use multiple cores and avoid one blocking call stopping every thread, but have more kernel-management overhead.

## 🧠 One-Minute Revision

```text
Program → passive
Process → executing program + resources/state
PCB → OS record for a process
Ready → waiting for CPU
Waiting → waiting for event/I-O
Thread → execution unit inside a process
Threads share heap/code/files, not stack/registers
```

> A process provides **isolation**; threads provide lighter-weight **concurrency** within it.
