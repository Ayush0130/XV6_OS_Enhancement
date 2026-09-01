# xv6 RISC-V Operating System Labs

## Author Details

- **Name:** Ayush Kumar
- **Roll Number:** 230123013

---

## 📌 Repository Overview

This repository contains comprehensive lab assignments and kernel-level implementations on the **MIT xv6 RISC-V** operating system. The projects span fundamental operating system topics including system call development, process scheduling, inter-process communication (IPC), advanced virtual memory management (Copy-on-Write and Demand Paging/Swapping), and file system enhancements (Doubly-Indirect Blocks and Symbolic Links).

---

## 📂 Repository Structure

| Directory | Lab Title | Key Topics Covered | Detailed Documentation |
| :--- | :--- | :--- | :--- |
| **`Lab_1/`** | **Basic Kernel Modifications & Syscalls** | Prime PID Allocation, Custom `top` System Call, Runtime Tick Tracking | [Lab 1 README](Lab_1/README.md) |
| **`Lab_2/`** | **Process Scheduling & Priority** | Weighted Round-Robin (WRR), Shortest Job First (SJF), Priority Syscalls | [Lab 2 README](Lab_2/README.md) |
| **`Lab_3/`** | **Inter-Process Communication (IPC)** | Shared Memory (`shm`), Mailboxes (`mbox`), Intertwined Memory Challenge | [Lab 3 README](Lab_3/README.md) |
| **`Lab_4/`** | **Advanced Virtual Memory** | Copy-on-Write (COW) Fork, MRU Page Replacement, Per-Process Swapping | [Lab 4 README](Lab_4/README.md) |
| **`Lab_5/`** | **File System Enhancements** | Doubly-Indirect Blocks (Large Files ~64 MiB), Symbolic Links (`symlink`) | [Lab 5 README](Lab_5/README.md) |

---

## 🛠️ Lab Summaries & Key Implementations

### 🔹 Lab 1: Basic Kernel Modifications & Custom System Calls
- **Task 1.1 — Prime PID Allocation:** Modified `allocpid()` in `kernel/proc.c` so that newly created user processes are uniquely assigned prime-numbered Process IDs (PIDs). Tested via `primepidtest.c`.
- **Task 1.2 — Custom `top` System Call:** Added system call `SYS_top` to monitor active processes, states (`RUNNING`, `SLEEPING`, etc.), process names, and CPU execution time (`rtime` in ticks). Tested via `top.c`.

### 🔹 Lab 2: Process Scheduling & Priority System Calls
- **Task 1.1 — Weighted Round-Robin (WRR) Scheduling:**
  - Implemented priority system calls: `set_priority()`, `get_priority()`, and `get_rtime()`.
  - Upgraded xv6 scheduler to allocate CPU time slices dynamically proportional to process priorities (`slice = priority`). Tested with `rrtest.c`.
- **Task 1.2 — Shortest Job First (SJF) Scheduling:**
  - Implemented non-preemptive Shortest Job First scheduling using estimated CPU bursts (`est_burst` / `remaining`).
  - Added `setburst()` syscall and dynamic arrival support tested via `arrival.c`.

### 🔹 Lab 3: Inter-Process Communication (IPC)
- **Task 1 — Kernel IPC Primitives:**
  - **Shared Memory:** Dynamic page allocation, reference counting, address space mapping/unmapping, and cleanup during process `exit()` and `exec()`.
  - **Mailboxes:** Ring buffer queue supporting synchronized blocking send and receive operations with spinlock protection. Tested with `shmmbox_test.c`.
- **Task 2 — Intertwined Memory Challenge:**
  - Concurrent multi-process coordination navigating shared memory paths using mailbox-based communication with asymmetric deadlock avoidance and termination handshake protocol (`master.c`, `process.c`).

### 🔹 Lab 4: Advanced Virtual Memory Management
- **Copy-on-Write (COW) Fork:**
  - Deferred physical page duplication on `fork()`. Pages are marked read-only with a custom COW flag.
  - Page-fault handler (`trap.c` / `vm.c`) performs on-demand duplication upon write attempts.
  - Thread-safe physical page reference counting (`incref`, `decref`, `getref`). Tested with `cowtest.c`.
- **Most Recently Used (MRU) Paging & Swapping:**
  - Implemented MRU doubly-linked list tracking for user pages.
  - Added per-process swap files (`/swap.<pid>`) to evict pages on physical memory pressure and handle swap-in page faults. Tested with `mrumem.c`.
  - Paging statistics interface via `getpagestat()` and `dumpmru()`.

### 🔹 Lab 5: File System Enhancements
- **Large File Support (Doubly-Indirect Blocks):**
  - Expanded xv6 maximum file size from ~70 KiB to **~64.3 MiB (65,803 blocks)** by adding a doubly-indirect block pointer to inode structure.
  - Updated `bmap()` and `itrunc()` for multi-level indirect block allocation and deallocation. Tested with `bigfile.c`.
- **Symbolic Links (Soft Links):**
  - Added `T_SYMLINK` inode type and `symlink(target, path)` system call.
  - Modified `open()` to recursively resolve symbolic links with loop detection (up to 10 depth levels) and support for `O_NOFOLLOW`. Tested with `symlinktest.c`.

---

## ⚙️ Prerequisites & Setup

To compile and run xv6 across any of the labs, ensure you have the RISC-V toolchain and QEMU installed:

### Linux (Ubuntu/Debian)
```bash
sudo apt-get update
sudo apt-get install git build-essential gdb-multiarch gcc-riscv64-linux-gnu binutils-riscv64-linux-gnu qemu-system-misc
```

### macOS (Homebrew)
```bash
brew tap riscv-software-src/riscv
brew install riscv-tools qemu
```

---

## 🚀 Building & Running

Navigate to any lab directory and launch the xv6 environment using QEMU:

```bash
# Example: Running Lab 1
cd Lab_1
make clean
make qemu
```

To exit QEMU at any time, press:
`Ctrl + A` then `X`

### Running Lab Test Commands

Inside the xv6 shell:

- **Lab 1:** `primepidtest`, `top`
- **Lab 2:** `rrtest`, `arrival 90 0 40 10 5 3`
- **Lab 3:** `shmmbox_test`, `master`
- **Lab 4:** `cowtest`, `mrumem`
- **Lab 5:** `bigfile`, `symlinktest`

---

## 📖 References

1. **xv6: a simple, Unix-like teaching operating system** — Russ Cox, Frans Kaashoek, Robert Morris.
2. **RISC-V Instruction Set Manual & Privileged Architecture Specification**.
3. **Operating System Concepts** — Silberschatz, Galvin, Gagne.
