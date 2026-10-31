---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "Demand paging with sparse hierarchical or inverted mappings, protection bits and a TLB; finite resources still limit admitted processes and working sets."
sources: ["Tanenbaum MOS 4e, sec. 3.3 (virtual memory: page tables, TLBs, inverted page tables)"]
changes:
  - "2026-10-31: Placed the short index at the root in the full-width illustrative page-table split."
  - "2026-10-31: Corrected 64-bit paging depth and finite-resource claims."
---
**Requirements:** 8 GB of physical memory, 64-bit virtual addresses, supporting many processes with large virtual spaces, with **protection** (a process cannot touch others) and **relocation** (a process can be loaded anywhere).

**Technique chosen: demand-paged virtual memory** with **hierarchical (multilevel) page tables** and a **TLB**.

- **Relocation:** the virtual address space of each process is divided into fixed-size pages (4 KB) that can be mapped to *any* frame; no contiguous memory is needed, so there is no external fragmentation and processes larger than RAM may run by keeping only their active pages resident and using backing storage.
- **Protection:** each process has its **own page table**, so it can only address the frames mapped into it; per-page bits (read/write/execute, user/supervisor) give fine-grained protection, and the page-table base register is writable only in kernel mode.
- **64-bit address space:** a single-level table is impossible ($2^{52}$ entries for 4 KB pages), so use a **multilevel table** (six levels of 7/9/9/9/9/9 VPN bits, plus a 12-bit offset, to cover all 64 virtual-address bits), whose lower levels are allocated only for the regions in use; or an **inverted page table** with one entry per physical frame ($8\text{ GiB}/4\text{ KiB}=2^{21}$ entries, about 32 MiB if each entry occupies an assumed 16 bytes), searched through a hash table, which has size independent of the number of processes.
- **Speed:** a **TLB** caches recent translations so most references need no table walk.

**Why not the alternatives:** base/limit registers need contiguous memory and suffer from fragmentation and swapping of whole processes; plain segmentation has external fragmentation. Paging gives a flexible solution. Four 9-bit levels plus a 12-bit offset cover only 48 bits, as in a common x86-64 configuration, not the full stated 64-bit space. No finite machine can guarantee an arbitrary number or size of processes: page tables, backing storage and working sets must fit their limits. On a single CPU execution is concurrent rather than physically simultaneous.
