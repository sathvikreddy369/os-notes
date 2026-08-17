# 7. File Systems and Storage

## 1. What Is a File System? ⭐⭐⭐

A **file system** organizes persistent data on storage devices and provides names, directories, permissions, and operations.

```text
Application → file API → file system → block device / disk
```

## 2. Files, Metadata and Operations

A file has data plus **metadata**:

```text
name | type | size | owner | permissions | timestamps | disk locations
```

Core operations:

```text
create | open | read | write | seek | close | delete | rename
```

`open()` generally returns a file descriptor/handle: a small process-local reference used by later operations.

## 3. Directories and Paths

A directory maps names to file metadata references.

```text
/
├── home/
│   └── user/
│       └── notes.md
└── etc/
```

```text
Absolute path → starts from root, e.g. /home/user/notes.md
Relative path → interpreted from current working directory
```

### Links and Permissions

```text
Hard link    → another directory entry for the same underlying file/inode
Symbolic link→ separate small file containing a path to a target
```

A hard link remains valid if another name is removed while the file still has links; a symbolic link can become dangling if its target disappears. Basic permissions control who may read, write, or execute a file/directory.

## 4. Access Methods

```text
Sequential access → read records in order
Direct/random access → jump to a position/block
Indexed access → use an index to locate data
```

## 5. File Allocation Methods

| Method | Idea | Main trade-off |
|---|---|---|
| Contiguous | file occupies adjacent blocks | fast access, external fragmentation/growth issue |
| Linked | each block points to next | no external fragmentation, poor random access |
| Indexed | index block holds block pointers | supports random access, index overhead |

Many real file systems use inode-like metadata with direct and indirect block pointers.

## 6. Inode and File Descriptor ⭐⭐

On Unix-like file systems:

```text
Directory entry → filename + inode number
Inode           → metadata + pointers to file blocks
File descriptor → per-process handle to opened file
```

> An inode stores file metadata, not the filename itself.

## 7. Free Space and Reliability

The file system tracks unused blocks using bitmaps, free lists, or related structures.

**Journaling** records intended metadata changes before applying them, helping recovery after a crash.

```text
Write intent to journal
       ↓
Apply actual change
       ↓
Mark journal transaction complete
```

Journaling improves consistency/recovery; it does not magically protect against every type of data loss.

## 8. Disk Basics and Scheduling

Storage is addressed as blocks/sectors. HDDs use rotating platters and a moving head; SSDs use flash memory, so they have no seek or rotational delay and are much faster for random access.

```text
Seek time       → move head to track
Rotational delay→ wait for sector (HDD)
Transfer time   → move data
```

Common HDD scheduling ideas:

```text
FCFS  → request order
SSTF  → nearest request first; may starve far requests
SCAN  → elevator: sweep in one direction, then reverse
C-SCAN→ sweep one direction; return without servicing
LOOK  → like SCAN, but reverse at last pending request, not physical end
C-LOOK→ like C-SCAN, but jump from last pending request to first pending request
```

SSDs have no rotating head, but OS I/O scheduling and queueing still matter.

**How to solve a disk-scheduling question:** draw the request positions and current head; follow the stated direction. Add absolute head movements between consecutive positions. For SCAN/C-SCAN, include the disk end only when the question's convention requires it; LOOK/C-LOOK stop at the last pending request.

## 9. RAID — Basic Idea

RAID combines disks for performance and/or redundancy.

```text
RAID 0 → striping; faster, no redundancy
RAID 1 → mirroring; redundancy, ~50% usable capacity
RAID 5 → striping + distributed parity; tolerates one disk failure
```

> RAID is not a backup: accidental deletion/corruption can be replicated too.

## 🧠 One-Minute Revision

```text
File system → names, directories, metadata, persistent blocks
File descriptor → process-local opened-file handle
Inode → metadata + data-block pointers
Contiguous/linked/indexed → allocation approaches
Journaling → crash-consistency aid
RAID ≠ backup
```
