# 5. Deadlocks

## 1. What Is Deadlock? ⭐⭐⭐

A **deadlock** is a permanent waiting situation: each process in a set waits for a resource/event that only another member of that set can cause.

```text
P1 holds R1, waits for R2
P2 holds R2, waits for R1
```

Neither can proceed.

## 2. Four Necessary Conditions ⭐⭐⭐

All four must hold for deadlock to be possible:

```text
1. Mutual exclusion  → a resource is non-shareable
2. Hold and wait      → hold one resource while requesting another
3. No preemption      → resource cannot be forcibly taken away
4. Circular wait      → closed chain of waiting processes
```

Break any one condition to prevent deadlock.

## 3. Resource-Allocation Graph (RAG)

```text
Process → Resource  means request
Resource → Process  means assignment
```

With one instance of every resource type:

```text
Cycle ⇔ deadlock
```

With multiple instances, a cycle indicates a possibility, not necessarily a deadlock.

## 4. Handling Strategies

```text
Ignore      → acceptable if extremely rare / recovery too costly
Prevention  → ensure at least one necessary condition never holds
Avoidance   → grant only requests that keep system safe
Detection   → allow deadlock, then detect and recover
```

### Prevention

Examples:

```text
Prevent hold-and-wait → request all resources at once
Prevent circular wait → impose global resource ordering
Allow preemption      → take some resources back when possible
```

Trade-off: reduced utilization/concurrency.

### Avoidance and Banker's Algorithm

Avoidance needs each process's declared **maximum possible demand**. A state is **safe** if some order exists in which every process can finish using currently available resources plus resources released by earlier finishers.

```text
Safe state   → no deadlock is guaranteed from current allocations
Unsafe state → not necessarily deadlocked; risk exists
```

Banker's algorithm grants a request only if the resulting state remains safe.

### Detection and Recovery

Detect cycles/waiting patterns, then recover by:

```text
Abort one or more processes
Preempt resources (when possible)
Roll back a process to a checkpoint
```

For a single instance of each resource type, cycle detection in the resource-allocation graph is enough. With multiple instances, use an availability/allocation check: repeatedly find a process whose remaining request can be satisfied, pretend it finishes and releases resources; processes left over are deadlocked.

## 5. Deadlock vs Starvation ⭐⭐⭐

```text
Deadlock   → involved processes cannot move; circular dependency
Starvation → a process waits indefinitely because others keep being favored
```

**Livelock:** processes keep changing state/responding to each other but make no useful progress.

## 🧠 One-Minute Revision

```text
Deadlock requires:
Mutual exclusion + hold-and-wait + no preemption + circular wait

Safe state ≠ currently deadlocked
Unsafe state ≠ necessarily deadlocked
Starvation ≠ deadlock
```

> The resource-ordering rule is a common practical way to prevent circular wait.
