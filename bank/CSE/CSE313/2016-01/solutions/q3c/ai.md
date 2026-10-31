---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "FIFO and standard Second Chance need no periodic tick work; NFU samples counts, aging shifts history, and WSClock sampling depends on its stated convention."
sources: ["Tanenbaum MOS 4e, sec. 3.4 (page replacement algorithms)"]
changes:
  - "2026-10-31: Separated standard Second Chance sweep updates from periodic sampled-reference variants."
---
| Algorithm | Tasks on each clock interrupt |
|:--|:--|
| **(i) FIFO** (replaces the oldest page) | **nothing**: it only depends on the load order (a queue of pages by arrival); no reference information is used |
| **(ii) Second chance** (FIFO with the R bit) | **No periodic action is required in the standard version**: when replacement examines the oldest page, an R=1 page has R cleared and moves to the back; an R=0 page is evicted. A periodically-cleared variant must be stated explicitly |
| **(iii) NFU** (not frequently used, a crude LRU) | for **every page add its R bit to its software counter** ($\text{counter}\mathrel{+}=R$), then **clear R** |
| **(iv) Aging** (a much better LRU approximation) | for every page **shift the counter right by one bit**, **insert the R bit at the leftmost position** ($\text{counter}=(\text{counter}\gg1)\,|\,(R\ll k-1)$), then **clear R** |
| **(v) WSClock** (working-set information) | **Account for process virtual time** and periodically sample/clear R as the implementation specifies. A common sampled variant timestamps R=1 pages before clearing; another refreshes timestamps while the replacement hand scans. Keep the convention consistent with the fault-time algorithm. |
