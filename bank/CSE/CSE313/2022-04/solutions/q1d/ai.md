---
author: ai
via: chat
status: unverified
summary: Hybrid threading multiplexes many user-level threads onto fewer kernel-level threads, combining fast user-space thread operations with the kernel's multi-CPU scheduling and non-blocking behaviour.
sources: [Process/Thread slides 39-40, 'Tanenbaum, MOS 4e, sec. 2.2.5']
imported_from: tfsolve-questions/data/solutions/2019-20.json
changes:
  - "2026-10-31: Clarified which user threads can run when a backing kernel thread blocks."
---
Hybrid = multiplex many user-level threads onto a smaller number of kernel-level threads.

- Combines cheap, fast user-level thread create/switch/synchronise (done in user space) with the kernel's ability to run threads on multiple CPUs and to keep the process running when one thread blocks (the blocked user thread remains waiting; other runnable user threads can execute on the remaining kernel threads, with run-time/kernel coordination as needed).
- The programmer can create very many user threads while the kernel manages only a few kernel threads, giving flexibility with modest kernel overhead.
