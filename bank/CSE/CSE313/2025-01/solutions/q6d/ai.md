---
author: ai
via: chat
status: unverified
imported_from: cse313_mem_fs_solved.tex
changes:
  - "2026-10-31: Clarified page-fault causes and avoided claiming all faults need disk I/O."
---
**Page fault:** a hardware exception when a virtual-memory access cannot complete using the current mapping, for example because the page is absent or access permissions forbid the operation. The trap hands control to the OS's fault handler, which locates the page's data, picks/evicts a physical frame if none is free, obtains its contents from disk or creates a zero-filled/copy-on-write page, updates the mapping and TLB as needed, and retries the instruction. An illegal access is rejected instead. A TLB miss alone is not necessarily a page fault.

**Thrashing:** the condition where the combined working sets of the currently-running processes exceed the amount of available physical memory. As a result, pages are constantly evicted and then almost immediately re-referenced (faulted back in), so affected processes spend much of their time waiting on page-fault-driven disk I/O rather than doing useful computation. The connection to page faults is direct: thrashing is precisely a state of an extremely high, sustained **page fault rate** -- the system is "busy" (disk light constantly on) but effective CPU utilization and throughput collapse, because faults recur fast enough that paging I/O dominates useful execution.
