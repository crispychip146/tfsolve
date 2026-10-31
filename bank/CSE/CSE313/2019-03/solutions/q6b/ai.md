---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "Independent blocking and multicore scheduling; costs are kernel bookkeeping and scheduling, while uncontended synchronization can stay in user space."
sources: ["Tanenbaum MOS 4e, sec. 2.2.4 (threads in the kernel)"]
changes:
  - "2026-10-31: Corrected all-operations-require-syscalls and fixed-thread-table claims."
---
**Advantages.** The kernel schedules threads independently, so a blocking I/O call normally blocks only its caller, while another ready sibling can run. Threads can execute on separate CPUs, and the kernel can preempt them and account for their CPU use separately.

**Costs.** Creating/destroying kernel threads and scheduling them requires kernel bookkeeping and per-thread kernel memory. Switches and blocking operations usually cost more than a purely user-space run-time switch. The application also has less control over the kernel's scheduling policy.

Do not infer that **every synchronization operation is a system call**: an uncontended mutex can acquire/release using user-space atomics, entering the kernel only when it needs to wait or wake another thread. A kernel thread table is not inherently a fixed-size array in every OS.

`fork()` and signal delivery require defined multithreading semantics. For example, POSIX `fork()` preserves only the calling thread in the child, though it copies the process's address space. These are design issues, not evidence that thread switching always flushes address mappings.
