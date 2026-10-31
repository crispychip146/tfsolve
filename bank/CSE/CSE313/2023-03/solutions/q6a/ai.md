---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "With immediate batch arrival and linear seek: SSTF 0.055 ms, SCAN 200.024 ms, C-SCAN 400.009 ms; LOOK/C-LOOK differ."
sources: ["OSTEP ch. 37 (hard disks, disk scheduling)", "Tanenbaum MOS 4e, sec. 5.4.3"]
changes:
  - "2026-10-31: Stopped seek accounting at the final serviced request of each batch and stated timing assumptions."
---
First translate block numbers into **tracks**, because seeking moves the head between tracks. Use binary units: $1\text{ GiB}/4\text{ KiB}=262144$ blocks. At 8 blocks per track, there are 32768 tracks, numbered 0 through 32767.

**Timing assumption.** A maximum seek time alone does not determine arbitrary seek times. For a numerical answer, assume seek time is linear in track distance:

$$t_{\text{track}}=\frac{100}{32767}\text{ ms}.$$

Assume SCAN and C-SCAN start upward, and a new batch arrives **immediately when the previous batch's last request is served**. Keep the current head position and direction between batches. Stop counting after the final request of batch 3; movement after that is not needed to handle these requests.

| Batch | Requested tracks, with duplicates retained |
|:-:|:--|
| 1 | 0, 5, 0, 1 |
| 2 | 1, 0, 1, 4, 0, 0 |
| 3 | 8, 6, 0, 0, 3 |

Multiple requests on the same track add no seek distance.

**(i) SSTF.** Always choose a pending track nearest to the current head:

| Batch | Head route | Distance |
|:-:|:--|:-:|
| 1 | 0, 1, 5 | 5 |
| 2 | 5, 4, 1, 0 | 5 |
| 3 | 0, 3, 6, 8 | 8 |

Total distance is 18 tracks, so **SSTF takes about 0.05493 ms** under the linear model.

**(ii) SCAN.** Reverse only at a physical end, rather than at the last pending request. Batch 1 finishes at track 5 while moving up. Batch 2 then arrives entirely behind the head, so it must first continue to the upper end and reverse.

| Batch | Head route | Distance |
|:-:|:--|:-:|
| 1 | 0, 1, 5 | 5 |
| 2 | 5, 32767, 4, 1, 0 | 65529 |
| 3 | serve track 0, reverse upward, then 3, 6, 8 | 8 |

$$T_{SCAN}=65542\frac{100}{32767}\text{ ms}\approx\mathbf{200.024\text{ ms}}.$$

**(iii) C-SCAN.** Serve while moving upward; return from the upper end to track 0 without serving during the return. Count the return as physical seeking.

| Batch | Head route | Distance |
|:-:|:--|:-:|
| 1 | 0, 1, 5 | 5 |
| 2 | 5, 32767, 0, 1, 4 | 65533 |
| 3 | 4, 6, 8, 32767, 0, 3 | 65533 |

$$T_{C\text{-}SCAN}=131071\frac{100}{32767}\text{ ms}\approx\mathbf{400.009\text{ ms}}.$$

**Other conventions.** If "elevator" means **LOOK**, which reverses at the last pending request, the total is 18 tracks, about 0.05493 ms. C-LOOK totals 29 tracks, about 0.08850 ms. If an exercise explicitly charges a *whole endpoint sweep after every batch*, including unnecessary motion after its last request, it instead gives 300 ms for SCAN or 600 ms for C-SCAN. That is a different completion/accounting convention. The paper does not state such an extra sweep.
