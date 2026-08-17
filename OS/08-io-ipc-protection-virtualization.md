# 8. I/O, IPC, Protection and Virtualization

## 1. I/O and Device Drivers

Programs should not need to know every device's hardware protocol. A **device driver** is OS software that translates generic OS requests into device-specific commands.

```text
Application
    ↓
OS I/O subsystem
    ↓
Device driver
    ↓
Controller / device
```

## 2. Polling, Interrupts and DMA

```text
Polling   → CPU repeatedly asks “are you done?”
Interrupt → device notifies CPU when it needs attention/completes
DMA       → device controller transfers a block directly between device and RAM
```

With DMA, CPU sets up the transfer and is typically interrupted on completion. This avoids CPU copying every byte itself.

## 3. Buffering, Caching and Spooling

```text
Buffering → temporary area to smooth speed/size mismatch during transfer
Caching   → keep copies of frequently/recently used data for faster reuse
Spooling  → queue work for a shared device, e.g. print jobs on disk
```

> A buffer mainly helps a transfer in progress; a cache mainly helps future reuse.

## 4. IPC — Interprocess Communication ⭐⭐

Processes are isolated, so they need explicit **IPC** to exchange data or coordinate.

| Mechanism | Core idea |
|---|---|
| Shared memory | processes map a common region; very fast but needs synchronization |
| Message passing | OS transfers messages; simpler isolation boundary |
| Pipe | byte stream, often parent-child/local pipeline |
| Named pipe (FIFO) | pipe with a filesystem name; unrelated local processes can use it |
| Message queue | kernel-managed discrete messages, often with ordering/priority support |
| Socket | communication endpoint; local or networked |
| Signal | lightweight asynchronous notification |

```text
Shared memory → fast data sharing, synchronization required
Message passing → kernel-mediated communication, simpler model
```

## 5. Client–Server and Sockets

```text
Client process  ↔  socket  ↔  network/local OS  ↔  socket  ↔  server process
```

A socket is an endpoint/API for communication. It is not the same as a WebSocket protocol.

## 6. Protection and Security ⭐⭐⭐

The OS enforces **protection**: which subject can access which object.

```text
Subject → user / process
Object  → file / memory / device / resource
Rights  → read / write / execute / delete
```

### Authentication vs Authorization

```text
Authentication → Who are you?
Authorization  → What are you allowed to do?
```

### Principle of Least Privilege

Give each user/process only the permissions it needs. This limits damage from bugs or compromise.

### Access Control

```text
ACL  → object stores who can do what (e.g., file permissions)
Capabilities → subject holds unforgeable permission/reference
```

## 7. Virtualization and Containers ⭐⭐

**Virtual machine (VM):** virtualizes hardware so each guest runs its own OS.

```text
Hardware → Hypervisor → Guest OS + apps (VM 1, VM 2, ...)
```

```text
Type 1 hypervisor → runs directly on hardware
Type 2 hypervisor → runs on top of a host OS
```

**Container:** isolates application environments while sharing the host kernel.

```text
Hardware → Host OS kernel → Containers + apps
```

| VM | Container |
|---|---|
| Includes guest OS | Shares host kernel |
| Stronger hardware-level isolation | Lighter/faster start |
| More resource overhead | Must use the host kernel family |

## 🧠 One-Minute Revision

```text
Driver → OS-to-device translator
Interrupt → device signals CPU
DMA → controller moves blocks between device and RAM
Buffer → smooth transfer
Cache → speed future reuse
IPC → communication between isolated processes
Named pipe / message queue → local IPC options beyond an anonymous pipe
VM → guest OS per virtual hardware
Container → isolated apps sharing host kernel
Type 1 / Type 2 → hypervisor on hardware / on a host OS
```
