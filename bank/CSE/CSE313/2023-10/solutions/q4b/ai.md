---
author: ai
via: chat
status: unverified
summary: 'Per-process (shared): Address Space, Global Variables, Child Processes, Directories. Per-thread (private): Program Counter, Registers, Stack, Local Variables.'
sources: ['Process/Thread slides 23, 41', 'Tanenbaum, MOS 4e, sec. 2.2.2 (Table 2-3)']
imported_from: tfsolve-questions/data/solutions/2021-22.json
changes:
  - "2026-10-31: Corrected cache/TLB flush claims and clarified thread-local variables."
---
A context switch saves the outgoing execution state and restores another task's state so it can resume at its own instruction. Both kinds must save registers, including the program counter and stack pointer; the important extra cost is whether the address space changes.

| Item | Between processes | Between threads of the same process |
|:--|:--|:--|
| Registers and stack | Save and restore each task's state | Save and restore each thread's state |
| Address space | Select the incoming process's page tables | Keep the shared page tables |
| TLB | May require invalidation, unless address-space tags preserve translations | Normally no invalidation due to address-space changes |
| Cache | Data normally stays cached, but the new process may have a different working set | Shared data may retain good locality |
| Typical cost | Higher | Lower |

A process switch does **not inherently flush the CPU's data or instruction caches**, and tagged TLB entries can survive it. A switch between threads belonging to *different* processes also changes the address space. Kernel threads require kernel scheduling; user-level thread switches can save and restore state inside the run-time library.

**Classification.** Address space, global variables, current/root directories and the child-process relationship belong to the process and are shared by its threads. Each thread has its own program counter, registers and stack, including its ordinary automatic local variables. A `static` local variable is shared storage; another thread can also access a stack object if its address is explicitly shared.
