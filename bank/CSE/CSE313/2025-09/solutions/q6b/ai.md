---
author: ai
via: chat
status: unverified
summary: "Some extra two-failure patterns are tolerated, but duplicate XOR parity does not give RAID-6 guarantees; capacity N-2 disks and backup-disk random-write bottleneck."
imported_from: cse313_mem_fs_solved.tex
changes:
  - "2026-10-31: Distinguished limited reliability improvement from guaranteed two-disk tolerance."
  - "2026-09-30 (import review): sequential-write throughput corrected from S to (N-2)*S. On full-stripe writes the backup disk writes one block per stripe, like every other disk, so it is not a bottleneck. Only small random writes are capped at R."
---
Setup: $N$ disks in total. $N-1$ of them form an ordinary RAID-5 (rotating parity); the last one is a fixed **backup-parity disk** that gets a second copy of every parity block. $S$ and $R$ are one disk's sequential and random throughput.

**(i) Reliability improves for some failure combinations, but there is no guarantee against any two failures.** The backup contains the same XOR parity equation, rather than an independent second equation:

- It survives any single failure, as RAID-5 does.
- It survives two failures only if one of them is the backup disk (or, stripe by stripe, if one failed disk held that stripe's parity).
- Two failed disks that both hold data for the same stripes still lose data, because each stripe still has just one parity equation.

For an ordinary RAID-5 inner array with at least three members, any two failed inner disks leave some stripes with two missing data blocks and only one independent parity equation. The array does, however, survive losing the backup disk together with any one inner disk: rebuild the inner disk using its ordinary RAID-5 parity. That is a limited reliability gain over an equally sized ordinary RAID-5, which cannot recover from any two whole-disk failures. RAID-6 supplies two independent equations and tolerates any pair.

**(ii) Effective capacity:** one disk's worth of rotating parity plus one backup disk, so $(N-2)$ disks of data, i.e. $\frac{N-2}{N}$ of raw capacity. That is one disk less than plain RAID-5 with the same $N$.

**(iii) Performance.** The inner RAID-5 has $N-1$ disks: sequential read and write $=(N-2)\,S$, random read $=(N-1)\,R$, random write $=\frac{N-1}{4}R$ (each small write is 4 I/Os: read old data and parity, write new data and parity).

- **Reads** never touch the backup disk: sequential $(N-2)\,S$, random $(N-1)\,R$. Unchanged.
- **Sequential (full-stripe) writes**: each stripe writes $N-2$ data blocks, 1 parity block and 1 backup-parity block, which is exactly **one block on every disk**, all in parallel. The backup disk does no more work than any other disk, so throughput stays $(N-2)\,S$.
- **Random (small) writes**: each needs the usual read-modify-write on its data and parity disks, *plus one write on the backup disk*. All small writes in the whole array land on that one disk, making it a hot spot like RAID-4's parity disk: random writes $\le R$. Together with the inner limit, $\min\!\big(\tfrac{N-1}{4}R,\;R\big)=R$ once $N\ge5$.

| | **Random** | **Sequential** |
|:--|:-:|:-:|
| **Read** | $(N-1)\cdot R$ | $(N-2)\cdot S$ |
| **Write** | $\min\{\tfrac{N-1}{4}R,\,R\} = R$ for $N\ge5$ | $(N-2)\cdot S$ |

**Conclusion for Alice.** RAID-5X costs one disk of capacity, tolerates some extra failure combinations but cannot promise to survive every pair, and turns the backup disk into a bottleneck for small random writes. With 9 disks, for example, plain RAID-5 gives $\tfrac{9}{4}R=2.25R$ of random writes; RAID-5X gives only $R$. Use RAID-6 instead.
