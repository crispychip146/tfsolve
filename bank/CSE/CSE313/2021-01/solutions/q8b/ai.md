---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "Original V7: 10 direct plus single/double/triple indirect, 512-byte blocks with 128 addresses per indirect block; tree capacity 1082201088 bytes."
sources: ["Tanenbaum MOS 4e, sec. 4.5.2 (the UNIX V7 file system)"]
changes:
  - "2026-10-31: Corrected original UNIX V7 block size and distinguished a hypothetical 1 KiB configuration."
---
The UNIX V7 file system keeps all the block addresses of a file in its **i-node**, which has **13 disk addresses**: the first **10 are direct**, the other 3 are **single, double and triple indirect**. Small files need no extra block; large files use pointer blocks.

![UNIX V7 i-node and its indirect blocks](figures/v7inode.png)

**Historical UNIX V7 uses 512-byte filesystem blocks.** With 4-byte addresses in indirect blocks, each holds $512/4=128$ addresses (the inode itself packs its on-disk addresses differently):

| Pointers | Blocks reached |
|:--|:-:|
| 10 direct | 10 |
| single indirect | $128$ |
| double indirect | $128^2 = 16{,}384$ |
| triple indirect | $128^3 = 2{,}097{,}152$ |
| **Total** | **2,113,674 blocks**, or **1,082,201,088 bytes** of addressable data |

To read block number $b$ of a file: if $b<10$ use the direct pointer; else if $b<10+128$ read the single-indirect block and use entry $b-10$; else use the double (and, for still larger $b$, the triple) indirect block, going one level deeper for each extra level. If a course example instead explicitly assumes 1 KiB blocks, the fan-out becomes 256 and the hypothetical limit is $(10+256+256^2+256^3)\times1024$ bytes. That is a different geometry; it should not be presented as the original V7 block size. The extra pointer blocks are read only for large files, and frequently used ones stay in the buffer cache.
