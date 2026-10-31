---
author: ai
via: chat
status: unverified
summary: "Process switches change address spaces; same-process thread switches retain them. Cache flushing is not inherent and tagged TLB entries may survive."
sources: ['Process/Thread slides 17, 25, 41', 'Tanenbaum, MOS 4e, sec. 2.2.2']
imported_from: tfsolve-questions/data/solutions/2023-24.json
changes:
  - "2026-10-31: Corrected unconditional TLB/cache flush claims and explained same-process thread switching."
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
