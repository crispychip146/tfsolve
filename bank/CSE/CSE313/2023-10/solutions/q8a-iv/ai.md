---
author: ai
via: chat
status: unverified
summary: "Using block striping and 15 ms positioning: RAID-0 segment writes about 810 MB/s; RAID-1 about 448 MB/s. Exact values depend on missing seek and stripe assumptions."
imported_from: cse313_mem_fs_solved.tex
changes:
  - "2026-10-31: Included rotational latency and explicit stripe/seek/unit assumptions in RAID throughput estimates."
  - "2026-09-30 (import review): RAID-1 segment write corrected. In OSTEP's RAID-1 model the 10 disks form 5 mirrored pairs and data is striped across the pairs, so sequential writes reach (N/2)*S, not a single disk's rate."
  - "2026-09-30 (import review): added a note on using the maximum vs. average seek time."
---
The file system reads one **4 KiB block** at a time but writes one **64 MiB segment**. RAID throughput depends on whether one request spans disks and whether several requests can be outstanding. A one-sector hardware transfer does **not** by itself specify the RAID stripe unit.

**Assumptions for a numerical illustration.** Use ten disks, block-sized striping, 1 KiB sectors transferred successively on each disk, and matching binary units for the stated MB/s. Treat RAID-1 as five mirrored pairs with data striped over pairs. Ignore caching, controller overhead and cleaning. All participating disks operate in parallel.

At 10000 RPM, one rotation is 6 ms, so average rotational latency is 3 ms. Only **maximum** seek time is given. For a conservative seek estimate, use 12 ms and thus positioning time $L=15$ ms. An actual average cannot be derived from that maximum alone.

**One random block read.** With block striping the 4 KiB block resides on one disk; four adjacent sectors share positioning. At $S=100$ MiB/s,

$$T_{read}=0.015+\frac{4/1024}{100}\text{ s}.$$

The useful rate for one outstanding read is about **260 KiB/s**. An isolated read has approximately the same latency in RAID-0 and mirrored RAID-1; the latter can choose either copy. With enough independent outstanding reads, both can approach ten times the per-disk random-read throughput. A serialized stream of one outstanding read cannot claim that aggregate ten-disk rate.

**One 64 MiB segment write.** RAID-0 sends $64/10=6.4$ MiB to each disk:

$$T_0=L+\frac{64}{10S}=0.079\text{ s},$$

$$R_0=\frac{64}{0.079}\approx\mathbf{810\text{ MiB/s}}.$$

With five mirrored pairs, each pair receives $64/5=12.8$ MiB; both copies are written in parallel:

$$T_1=L+\frac{64}{5S}=0.143\text{ s},$$

$$R_1=\frac{64}{0.143}\approx\mathbf{448\text{ MiB/s}}.$$

Both formulas include **seek plus rotation once per participating disk**, overlapped across disks. Omitting rotation from writes while including it in reads would be inconsistent. If positioning were negligible for a long stream, the rates approach $10S$ and $5S$ respectively.

**Limits of the data.** An illustrative average-seek assumption of $12/3=4$ ms gives $L=7$ ms and segment-write rates about **901** and **474 MiB/s** instead. If the RAID stripes at **sector** rather than block granularity, one block may span four disks and its single-read calculation changes. Consequently the paper does not determine one unique exact throughput without a seek model, stripe layout and concurrency assumption. Capacity, platter count and cache size do not supply those missing details.
