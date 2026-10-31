---
author: ai
via: chat
status: unverified
imported_from: cse313_mem_fs_solved.tex
changes:
  - "2026-10-31: Answered total physical memory as the primary result and separated page-table overhead."
  - "2026-09-30 (import review): added the reading where the process's own pages are counted too."
summary: "Assuming all 33 used pages resident: total memory 3136 B (single-level), 2432 B (two-level); table-only overhead 1024 B versus 320 B."
---
The question asks for the **process's total physical memory**, so count its resident data/code/stack pages **and** its page-table structures. Assume every listed used page is resident, no sharing, a 4-byte directory entry as well as PTE, and binary KB units.

There are $16384/64=256$ virtual pages. Each 64-byte page holds $64/4=16$ entries, so a page-sized two-level table uses 4 VPN bits for each level, followed by a 6-bit page offset.

**Resident process pages.** Code uses 2 pages, heap VPNs 4 through 32 use $32-4+1=29$ pages, and stack uses 2 pages. Thus

$$M_{data}=(2+29+2)\times64=2112\text{ B}.$$

The physical data-page cost is the same under either table layout; only the mapping overhead changes.

**(i) Single-level table.** It needs one PTE for each of the 256 possible virtual pages, even for unused addresses:

$$M_{PT}=256\times4=1024\text{ B}.$$

$$M_{total}=2112+1024=\boxed{3136\text{ B}}.$$

**(ii) Two-level table.** A leaf table maps 16 consecutive VPNs. The required page-directory indices are:

| Pages | Directory indices |
|:--|:--|
| Code 0-1 | 0 |
| Heap 4-32 | 0, 1, 2 |
| Stack 254-255 | 15 |

Code and part of the heap share the leaf for index 0; do not count that leaf twice. Exactly **four** leaf tables are needed, for indices 0, 1, 2 and 15. Together with one 16-entry directory, each occupying 64 bytes,

$$M_{tables}=(1+4)\times64=320\text{ B},$$

$$M_{total}=2112+320=\boxed{2432\text{ B}}.$$

**Comparison.** Total resident memory falls from **3136 B to 2432 B**, saving **704 B**. If only page-table storage is requested, the corresponding answers are **1024 B and 320 B**. If demand paging leaves some listed process pages nonresident, their data-frame cost would depend on residency; the table comparison above still follows from the mappings assumed in the question.
