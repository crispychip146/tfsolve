---
author: ai
via: chat
status: unverified
imported_from: cse313_mem_fs_solved.tex
changes:
  - "2026-10-31: Used coherent TLB invalidation as the main model and added the actual Clock/TLB trace."
  - "2026-09-30 (import review): stated the pure-LRU TLB assumption and the result with TLB invalidation on eviction."
---
Reference string: $1,2,3,4,5,2,3,1,2,3,4,5,1$ (13 references).

**(i) Belady's anomaly.** It is the counter-intuitive result that, for some replacement policies (notably FIFO), *increasing* the number of available page frames can *increase* the number of page faults, instead of decreasing or staying the same as one would expect. (Policies such as LRU/Optimal are "stack algorithms" and never exhibit this; only non-stack algorithms like FIFO can.)

*FIFO with 3 frames* (evict oldest):

|  Ref   |  1  |  2  |  3  |  4  |  5  |  2  |  3  |  1  |  2  |  3  |  4  |  5  |  1  |
|:------:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Fault? |  F  |  F  |  F  |  F  |  F  |  F  |  F  |  F  |  H  |  H  |  F  |  F  |  H  |

Total faults with 3 frames $=\mathbf{10}$ (hits only at the 9th,10th,13th references).

*FIFO with 4 frames*:

|  Ref   |  1  |  2  |  3  |  4  |  5  |  2  |  3  |  1  |  2  |  3  |  4  |  5  |  1  |
|:------:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Fault? |  F  |  F  |  F  |  F  |  F  |  H  |  H  |  F  |  F  |  F  |  F  |  F  |  F  |

Total faults with 4 frames $=\mathbf{11}$ (hits only at the 6th,7th references).

Since faults *increased* from 10 (3 frames) to 11 (4 frames) under FIFO, **Belady's anomaly is demonstrated** for this reference string at cache sizes 3 and 4.

**(ii) Optimal algorithm hit/miss rate.** (Evict the page whose next use is farthest in the future / never used again.)

*Optimal, 3 frames*: faults at refs 1,2,3,4,5,8,11,12 $\Rightarrow$ **8 faults**, 5 hits (refs 6,7,9,10,13). Hit rate $=5/13\approx38.5\%$, miss rate $=8/13\approx61.5\%$.

*Optimal, 4 frames*: faults at refs 1,2,3,4,5,11 $\Rightarrow$ **6 faults**, 7 hits. Hit rate $=7/13\approx53.8\%$, miss rate $=6/13\approx46.2\%$.

Note Optimal's fault count *decreases* (8$\to$ 6) as frames increase from 3 to 4 -- the opposite of FIFO above -- confirming that Optimal (like LRU) is a stack algorithm immune to Belady's anomaly.

**(iii) TLB (4-entry, LRU) + CLOCK (4-frame page cache) memory access time.** Using the given hint table, the page-cache CLOCK simulation for the 13 references yields **8 page-cache faults** (refs 1,2,3,4,5,8,11,12) and **5 page-cache hits** (refs 6,7,9,10,13) -- worked out by extending the given first-5-access table forward with the same sweep-and-second-chance rule.

A TLB caches **translations**, not page contents. Whenever CLOCK evicts a page, invalidate its TLB translation before reusing its frame; otherwise a TLB hit could access the wrong page. Apply LRU only among the still-valid TLB entries. Assume one-level page-table walks, clean victims, and one disk read per page fault. Charge one TLB lookup, one PTE lookup and one data access on a fault, excluding additional fault-handler/retry overhead.

Here `*` marks the frame just filled; the next fault searches after it. Bits are in frame order; hits do not move the hand.

| Ref | Page | Frames after access | R bits | Just-filled frame | TLB result |
|:-:|:-:|:--|:-:|:-:|:--|
| 1 | 1 | 1, --, --, -- | 1000 | 1 | miss + fault |
| 2 | 2 | 1, 2, --, -- | 1100 | 2 | miss + fault |
| 3 | 3 | 1, 2, 3, -- | 1110 | 3 | miss + fault |
| 4 | 4 | 1, 2, 3, 4 | 1111 | 4 | miss + fault |
| 5 | 5 | 5, 2, 3, 4 | 1000 | 1 | miss + fault |
| 6 | 2 | 5, 2, 3, 4 | 1100 | 1 | hit |
| 7 | 3 | 5, 2, 3, 4 | 1110 | 1 | hit |
| 8 | 1 | 5, 2, 3, 1 | 1001 | 4 | miss + fault |
| 9 | 2 | 5, 2, 3, 1 | 1101 | 4 | hit |
| 10 | 3 | 5, 2, 3, 1 | 1111 | 4 | hit |
| 11 | 4 | 4, 2, 3, 1 | 1000 | 1 | miss + fault |
| 12 | 5 | 4, 5, 3, 1 | 1100 | 2 | miss + fault |
| 13 | 1 | 4, 5, 3, 1 | 1101 | 2 | hit |

At reference 12, evicting page 2 removes its translation, leaving space for page 5 without evicting page 1's TLB entry. Reference 13 is therefore a **TLB hit**. The costs are:

- References 1--5: TLB miss *and* page fault (cold start) -- cost each $=$ TLB(5ns) $+$ page-table walk(60ns) $+$ disk(4,000,000ns) $+$ memory(60ns) $=\mathbf{4{,}000{,}125}$ns.

- References 6,7,9,10,13: page present, VPN still resident in the TLB $\to$ **TLB hit**, cost $=5+60=\mathbf{65}$ns each.

- References 8,11,12: page evicted from the cache earlier $\to$ TLB miss *and* page fault again, cost $=\mathbf{4{,}000{,}125}$ns each.

There are **8 page faults and 5 TLB hits**, with no resident-page TLB misses in this trace.

$$T=8(4000125)+5(65)=\mathbf{32001325\text{ ns}}.$$

That is **32.001325 ms total**, or approximately **2.46164 ms per reference**. Disk service dominates: $8\times4$ ms already accounts for 32 ms.

If one simulates an independent four-entry LRU cache of VPN names without invalidating translations on frame reuse, reference 13 instead misses and adds 60 ns. That gives 32001385 ns, but is not a coherent TLB model and should not be the primary answer.
