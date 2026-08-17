# 3. CPU Scheduling

## 1. Why Scheduling? ⭐⭐⭐

When more ready processes exist than CPU cores, the scheduler chooses the next one to run.

```text
Ready queue → Short-term scheduler → CPU
```

**Preemptive:** OS may take CPU from a running process.  
**Non-preemptive:** process keeps CPU until it exits or blocks.

## 2. Scheduling Criteria

```text
CPU utilization → keep CPU busy
Throughput       → jobs completed per time
Turnaround time  → completion time − arrival time
Waiting time     → total time spent in ready queue
Response time    → first CPU/service time − arrival time
```

For interactive systems, response time matters greatly. For batch work, throughput/turnaround often matter more.

## 3. FCFS — First Come, First Served

Runs processes in arrival order. It is non-preemptive.

```text
P1 (long) → P2 (short) → P3 (short)
```

Simple and fair by arrival, but may cause the **convoy effect**: short jobs wait behind a long CPU-bound job.

## 4. SJF and SRTF ⭐⭐⭐

**SJF (Shortest Job First):** choose smallest next CPU burst; typically non-preemptive.  
**SRTF (Shortest Remaining Time First):** preemptive SJF; a new shorter remaining job can preempt.

SJF minimizes average waiting time **if burst lengths are known**. In reality, OS estimates them from past behavior.

```text
SJF  → shortest total next burst
SRTF → shortest remaining time; can preempt
```

Risk: long jobs can starve if short jobs keep arriving.

## 5. Priority Scheduling

Run the highest-priority process first. It can be preemptive or non-preemptive.

Risk: **starvation** of low-priority tasks.

**Aging:** gradually increase a waiting task's priority to prevent starvation.

## 6. Round Robin (RR) ⭐⭐⭐

Each ready process gets a fixed **time quantum** in circular order.

```text
P1 → P2 → P3 → P1 → ...
```

Good for time-sharing and responsiveness.

```text
Very large quantum → behaves like FCFS
Very small quantum → too many context switches
```

## 7. Multilevel Queue and Feedback Queue

**Multilevel queue:** separate fixed queues, e.g. foreground interactive vs background batch; queues may have different policies.

**Multilevel feedback queue (MLFQ):** processes may move between queues. Interactive/short tasks tend to stay high; CPU-heavy tasks move lower. This approximates SJF without knowing burst times in advance.

## 8. Scheduling Calculation Pattern

For each process, draw a Gantt chart, then compute:

```text
Turnaround = Completion − Arrival
Waiting    = Turnaround − CPU burst
Response   = First start − Arrival
```

> For RR, include every preemption slice in the Gantt chart before calculating.

## 🧠 One-Minute Revision

```text
FCFS → arrival order; convoy effect
SJF  → shortest burst; optimal average wait if burst known
SRTF → preemptive SJF
Priority → starvation; solve with aging
RR → time quantum; interactive systems
```

> **Response time is not turnaround time.** Response ends at first service; turnaround ends at completion.
