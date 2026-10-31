---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "Context switches save execution state; changing address spaces may add translation and working-set costs. Same-process threads avoid the address-space change."
sources: ["Tanenbaum MOS 4e, sec. 2.1.6 and 2.2 (context switching, threads)"]
changes:
  - "2026-10-31: Qualified TLB/cache context-switch costs and same-process thread savings."
---
**Why a (process) context switch is expensive.**

- It needs a **trap into the kernel**, saving the old process's registers, program counter and status in its PCB, and running the **scheduler** to choose the next process.
- The kernel must **switch the address space**: select the new page tables and address-space tag. Untagged TLB entries may need invalidation; tagged entries can remain valid for later reuse.
- **Caches** (instruction and data caches, branch predictors) contain the old process's data, so a different working set may cause extra cache misses. A switch does not inherently clear all caches; pipeline and predictor costs depend on the architecture.
- The new process's registers are restored and the mode is switched back to user mode.

**How threads help.** Threads of the same process **share the address space** (code, data, heap, open files). A switch between two threads of one process saves and restores its execution state, including registers, program counter and stack pointer: **no change of the page tables, no TLB flush, and shared cached data may retain good locality**. With user-level threads, the switch is even a procedure call in user space (no trap). So thread switching is much cheaper than process switching (and communication between threads uses shared memory directly).
