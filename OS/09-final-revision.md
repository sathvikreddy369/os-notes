# 9. OS Final Revision and Interview Questions

## 1. Process vs Thread ⭐⭐⭐

```text
Process → own address space/resources; stronger isolation; heavier
Thread  → execution unit within process; shares memory/files; lighter
```

Threads share code, heap, and open files, but each needs its own stack, registers, and program counter.

```text
Concurrency → overlapping progress; possible on one CPU
Parallelism  → simultaneous execution; needs multiple execution resources
```

## 2. Program vs Process ⭐⭐⭐

```text
Program → passive executable/instructions on disk
Process → executing program with runtime state/resources
```

## 3. Process States ⭐⭐⭐

```text
Ready   → eligible to run; waiting for CPU
Running → executing now
Waiting → blocked for I/O/event
```

## 4. Context Switch ⭐⭐

OS saves one task's CPU state and restores another's. It enables multitasking but adds overhead.

## 5. Scheduling ⭐⭐⭐

```text
FCFS → simple; convoy effect
SJF  → lowest average wait if burst known
SRTF → preemptive SJF
Priority → starvation possible; aging helps
RR   → time quantum; good responsiveness
```

```text
Turnaround = completion − arrival
Waiting    = turnaround − CPU burst
Response   = first start − arrival
```

```text
AT → arrival time     BT → CPU burst time     CT → completion time
CPU burst → executing   I/O burst → waiting for an I/O event
```

For calculation questions: make the Gantt chart first, then calculate each process's CT, TAT, WT, and response time. In RR, requeue unfinished work after each quantum.

## 6. Race Condition, Mutex and Semaphore ⭐⭐⭐

```text
Race condition → timing-dependent incorrect shared-data result
Mutex          → exclusive ownership lock
Semaphore      → atomic counter/signaling mechanism
```

```text
Mutex wait → generally blocks
Spinlock wait → busy-waits
```

## 7. Deadlock ⭐⭐⭐

```text
Mutual exclusion
Hold and wait
No preemption
Circular wait
```

All four are necessary for deadlock. Prevent it by breaking at least one; resource ordering breaks circular wait.

```text
Deadlock   → no involved process can proceed
Starvation → one process repeatedly loses scheduling/resource chance
Livelock   → activity continues but no useful progress
```

## 8. Paging and Virtual Memory ⭐⭐⭐

```text
Virtual address → page table / TLB → physical frame
```

```text
Page → fixed-size virtual-memory block
Frame → fixed-size physical-memory block
TLB → cache of address translations
```

```text
TLB miss  → translation not cached; page may still be in RAM
Page fault→ referenced page absent from RAM; OS must load it
```

```text
Page fault flow → validate reference → find/replace frame → disk read → update mapping → restart instruction
```

### Contiguous Allocation ⭐⭐

```text
Internal fragmentation → waste inside allocated block
External fragmentation → free space split into holes
First fit → first adequate hole
Best fit → smallest adequate hole
Worst fit → largest adequate hole
```

## 9. Page Replacement ⭐⭐

```text
FIFO    → oldest page; Belady's anomaly possible
Optimal → farthest future use; benchmark only
LRU     → least recent use; locality-based
```

**Thrashing:** heavy page faults leave little time for useful execution.

For replacement calculations, show frames after every reference. FIFO evicts oldest-loaded; LRU evicts least-recently used; optimal evicts the page used farthest in the future.

## 10. File System ⭐⭐

```text
File descriptor → process-local handle to an opened file
Inode           → file metadata + pointers to data blocks
Directory       → maps names to file metadata references
```

```text
Hard link → another name for the same inode/file
Soft link → path-based reference; may become dangling
```

```text
RAID improves availability/performance in some configurations
RAID does not replace backup
```

## 11. I/O and IPC ⭐⭐

```text
Interrupt → device signals CPU
DMA → device controller transfers data directly to RAM
Shared memory → fast IPC but needs synchronization
Message passing → explicit OS-mediated communication
```

```text
Pipe → local byte stream
Named pipe → pipe usable by unrelated local processes
Message queue → discrete kernel-managed messages
Socket → local or network communication endpoint
```

## 12. Disk Scheduling and Virtualization ⭐

```text
SSTF  → nearest request; starvation possible
SCAN  → sweep then reverse
C-SCAN→ one-direction service, then return
LOOK/C-LOOK → stop at last request rather than disk end
```

```text
Type 1 hypervisor → directly on hardware
Type 2 hypervisor → runs on host OS
VM → guest OS per virtual hardware
Container → isolated app environment sharing host kernel
```

## 13. Common Interview Answers

### What is a system call?

> A controlled request from a user-space program to the kernel for services such as file I/O, process creation, memory mapping, or networking.

### Why are threads faster than processes?

> They share their process's address space/resources, so creation and switching generally require less work. The trade-off is weaker isolation and synchronization complexity.

### Why is a context switch expensive?

> CPU state must be saved/restored; caches and address-translation state may also become less useful. No application work occurs during the switch.

### What is a page fault?

> A trap caused when a process references a virtual page not currently present in RAM. The OS validates it, loads the page if valid, and resumes the instruction.

### Why does virtual memory exist?

> To provide isolation, flexible address spaces, and the illusion of more usable memory than RAM by loading needed pages on demand.

### Mutex vs semaphore?

> A mutex is an ownership-based mutual-exclusion lock. A semaphore is a counter/signaling primitive and can represent multiple available resources.

### TLB miss vs page fault?

> A TLB miss means a translation was not in the fast TLB cache; the page table may still show that the page is in RAM. A page fault means the referenced page is absent from RAM.

## 🔥 Final OS Priority List

### 🔴 High Priority — Know Very Well

1. **Kernel vs user mode; system calls**
2. **Program vs process; process states; PCB**
3. **Process vs thread; context switching**
4. **FCFS, SJF/SRTF, priority, RR**
5. **Race condition, critical section, mutex, semaphore**
6. **Deadlock conditions, prevention, starvation**
7. **Paging, TLB, virtual memory, page fault**
8. **Page replacement and thrashing**
9. **File descriptor, inode, basic file system concepts**

### 🟡 Medium Priority — Know Conceptually

10. Monolithic vs microkernel
11. User vs kernel threads
12. Monitors and classical synchronization problems
13. Banker's algorithm / safe state
14. Segmentation, multilevel paging, copy-on-write
15. Disk scheduling, journaling, RAID basics
16. DMA, buffering/caching/spooling, IPC
17. Disk scheduling; virtual machines vs containers

### 🟢 Low Priority — Do Not Overinvest

18. Exact kernel data structures
19. Full Banker's-algorithm arithmetic unless your exam demands it
20. Exact page-table formats / CPU-specific registers
21. Detailed file-system implementation internals
22. Enterprise storage and RAID controller details

---

## ✅ OS General Basics: Complete

You now have the OS knowledge expected for regular CS interviews and placements, without going deep into OS-kernel development. Revise the 🔴 topics frequently, then practice a few scheduling and page-replacement calculations separately.
