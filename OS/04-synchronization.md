# 4. Process Synchronization

## 1. Race Condition and Critical Section ⭐⭐⭐

A **race condition** occurs when the result depends on timing/interleaving of concurrent operations.

```text
balance = 100
Thread A reads 100       Thread B reads 100
A writes 90              B writes 80
Final = 80  (one update lost; expected 70)
```

The code that accesses shared data is the **critical section**.

```text
Entry section → Critical section → Exit section → Remainder section
```

A correct solution aims for:

```text
Mutual exclusion → at most one inside critical section
Progress         → a decision cannot be postponed forever unnecessarily
Bounded waiting  → no thread waits forever while others repeatedly enter
```

## 2. Mutex and Spinlock ⭐⭐⭐

A **mutex** is a lock with one owner.

```text
lock()
  critical section
unlock()
```

If unavailable, the contender usually sleeps/blocks rather than consuming CPU.

A **spinlock** repeatedly checks the lock (busy-waits). It is useful only for very short waits, especially in low-level/kernel code where sleeping may be impossible.

```text
Mutex    → block/sleep while waiting
Spinlock → keep CPU busy while waiting
```

## 3. Peterson's Solution — Concept Only

Peterson's algorithm is a classic software-only solution for **two** processes. Each process announces interest and gives the other process a turn when both want to enter.

It illustrates mutual exclusion, progress, and bounded waiting under idealized assumptions.

> Know it for theory/MCQs, but do not use it in production: modern CPUs/compiler memory reordering and its two-process limitation make hardware-backed locks the practical choice.

## 4. Semaphores ⭐⭐⭐

A semaphore is an integer synchronization primitive operated atomically.

```text
wait(P/down)   → decrement; block if unavailable
signal(V/up)   → increment; wake one waiter if needed
```

```text
Binary semaphore    → 0 or 1; can implement mutual exclusion
Counting semaphore  → number of available identical resources
```

Example: 3 database connections → semaphore initially `3`.

> A mutex has ownership; a semaphore is a counter/signaling primitive. Do not treat them as identical.

## 5. Monitor and Condition Variable

A **monitor** packages shared data and operations so only one thread executes monitor code at a time. A **condition variable** lets a thread wait for a condition such as “buffer not empty.”

```text
while (buffer is empty)
    wait(notEmpty)     // releases monitor lock while waiting

consume item
signal(notFull)
```

Use `while`, not `if`: a thread must recheck the condition after waking.

## 6. Classical Problems

### Producer–Consumer / Bounded Buffer ⭐⭐⭐

```text
Producer → [ finite buffer ] → Consumer
```

Need:

```text
mutex  → protects buffer data
empty  → number of empty slots
full   → number of filled slots
```

Producer waits for `empty`; consumer waits for `full`.

### Readers–Writers

Many readers may read together; a writer needs exclusive access.

```text
Readers can share read access
Writer cannot overlap with readers or writers
```

Design choice: reader preference can starve writers; writer preference can delay readers.

### Dining Philosophers

Each philosopher needs two forks. Naively grabbing one then waiting for the other can cause deadlock.

Typical fixes: impose fork ordering, allow at most `N−1` philosophers to try, or use a monitor.

## 7. Atomic Operations

Hardware instructions such as compare-and-swap can update a value only if it still equals an expected value. They underpin locks.

```text
if value == expected:
    value = new
    success
```

## 🧠 One-Minute Revision

```text
Race condition → unsafe timing-dependent shared-data access
Critical section → code touching shared resource
Mutex → exclusive owner lock
Semaphore → atomic counter / signaling
Spinlock → busy wait; only short waits
Monitor + condition → structured shared-state synchronization
```

> **Concurrency bugs can happen even on one CPU** because threads may be interleaved.
