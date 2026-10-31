---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "Move next-block pointers into a cached FAT; chain traversal uses RAM, though it remains linear in block index."
sources: ["Tanenbaum MOS 4e, sec. 2.4.5 (lottery scheduling) and 2.4.2"]
changes:
  - "2026-10-31: Replaced unrelated scheduling notes with the FAT-in-memory modification and its random-access cost."
---
With ordinary linked allocation, a data block contains both file bytes and the address of the next block. To reach logical block $k$, the OS must read preceding blocks just to discover their successors. Random access therefore causes up to $k$ dependent disk reads before reaching the target.

Move those successor addresses into a **file allocation table (FAT)** indexed by physical block number, and keep the table, or the relevant parts of it, in RAM. The directory entry still stores the first block.

For an illustrative file occupying blocks 7, 21 and 9:

| FAT index | Next block |
|:-:|:--|
| 7 | 21 |
| 21 | 9 |
| 9 | EOF |

To read logical block 2 (zero-based), start at 7, follow `FAT[7]` to 21, then `FAT[21]` to 9, and read block 9. The two chain steps are now RAM accesses; the earlier data blocks need not be read from disk. Removing the links from data blocks also leaves their full capacity available for file bytes.

**Limit:** FAT improves the cost of each chain step, but does not eliminate the chain: finding logical block $k$ still takes $O(k)$ table steps. An additional per-file index, cached chain checkpoints, or an inode with direct/indirect block pointers can avoid walking from the beginning. Caching the FAT costs RAM and its on-disk copy must remain consistent.
