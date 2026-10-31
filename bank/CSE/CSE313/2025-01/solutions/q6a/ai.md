---
author: ai
via: chat
status: unverified
imported_from: cse313_mem_fs_solved.tex
changes:
  - "2026-10-31: Made the surviving two-disk failure combinations explicitly cross-group."
  - "2026-10-31: Corrected comparison with single-parity RAID-4 capacity and throughput."
  - "2026-09-30 (import): split a display equation that ran off the page; content unchanged."
---
Let there be $N$ total disks, 2 of which are dedicated parity disks: $P_{odd}$ holds the XOR-parity of all odd-numbered data disks, and $P_{even}$ holds the XOR-parity of all even-numbered data disks. The remaining $N-2$ disks are data disks, split roughly evenly between the two groups.

**Storage capacity.** Two whole disks are reserved for parity, so usable capacity $=(N-2)$ disks, i.e. a fraction $\frac{N-2}{N}$ of the raw total -- one disk less usable capacity than ordinary RAID-4 with the same total disk count, which reserves only one parity disk.

**Reliability.** Any single disk failure is always tolerated (the failed disk's group parity reconstructs it). Beyond that, the design can *also* survive certain simultaneous 2-disk failures: e.g. one odd-group member and one even-group member failing together, where a member may be a data disk or that group's parity disk, since these are two independent single-parity sub-arrays. It **cannot** survive 2 failures *within the same group* (e.g. two odd disks, or an odd disk together with $P_{odd}$), since a group only has single-parity redundancy. So reliability is improved over plain RAID-4 (which fails on *any* 2 simultaneous failures), but it is still weaker than RAID-6 (which tolerates *any* 2 failures).

**Throughput.** Let $S$ = one disk's sequential throughput, $R$ = one disk's random-I/O throughput. Model the array as **two independent, parallel RAID-4 sub-arrays**, each of size $N/2$ (an odd-group array of $N/2-1$ data disks $+P_{odd}$, and an even-group array of $N/2-1$ data disks $+P_{even}$). Applying the standard RAID-4 formulas to each sub-array and summing (since the two groups use entirely disjoint disks and can be driven fully in parallel):

$$\text{Seq R} = \text{Seq W} = \Big(\tfrac{N}{2}-1\Big)S \qquad \text{(per group)}$$

$$\text{Rand R} = \Big(\tfrac{N}{2}-1\Big)R, \qquad \text{Rand W} = \tfrac{R}{2} \qquad \text{(per group)}$$

(the $R/2$ is RAID-4's classic small-write bottleneck: every random write forces a read-old-parity + write-new-parity pair on that group's *one* dedicated parity disk). Summing the two identical, independent groups:

$$\text{Seq R} = \text{Seq W} = 2\Big(\tfrac{N}{2}-1\Big)S = (N-2)S, \qquad \text{Rand R} = (N-2)R, \qquad \text{Rand W} = 2\times\tfrac{R}{2} = R.$$

|  | **Random** | **Sequential** |
|:---|:--:|:--:|
| **Read** | $(N-2)\cdot R$ | $(N-2)\cdot S$ |
| **Write** | $R$ $\big(=2\times\tfrac{R}{2}$, one bottleneck per group, run in parallel$\big)$ | $(N-2)\cdot S$ |

**Interpretation**: sequential and random *read* throughput are lower than plain single-parity RAID-4 with the same total disk count: the useful data-disk count drops from $N-1$ to $N-2$. Sequential *write* likewise drops from $(N-1)S$ to $(N-2)S$ (no read-modify-write penalty for full-stripe writes). The real difference is *random write*: ordinary single-parity RAID-4 caps at $R/2$ *system-wide* (one parity disk services every write in the whole array); here, because odd- and even-group writes each hit a *different* dedicated parity disk, the two groups' $R/2$ bottlenecks run independently and add up, giving $R$ overall -- **double** the random-write throughput of plain RAID-4 for the same disk count (though writes concentrated entirely within one group still bottleneck at that group's own $R/2$).
