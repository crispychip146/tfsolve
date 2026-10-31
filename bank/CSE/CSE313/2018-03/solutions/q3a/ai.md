---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "Tree capacity 141012904202240 bytes (about 128.25 TiB); 32-bit block numbers limit addressable disk space to 8 TiB including metadata."
sources: ["Bach, ch. 4 (inode block addressing)", "Tanenbaum MOS 4e, sec. 4.3.2"]
changes:
  - "2026-10-31: Distinguished logical sparse-file limits from fully allocated data capacity."
  - "2026-10-31: Distinguished pointer-tree capacity from the 32-bit physical block-address limit."
---
A block is $2\text{ K}=2048$ bytes and a block address needs $32\text{ bits}=4$ bytes, so a block of pointers holds $2048/4=512$ addresses. The modified table of contents has 14 entries: **10 direct**, and single, double, triple and quadruple indirect.

| Entry | Data blocks addressed |
|:--|:-:|
| 10 direct | 10 |
| single indirect | $512$ |
| double indirect | $512^2=262{,}144$ |
| triple indirect | $512^3=134{,}217{,}728$ |
| quadruple indirect | $512^4=68{,}719{,}476{,}736$ |
| **Total** | **68,853,957,130** blocks |

$$68{,}853{,}957{,}130\times2048\text{ B}=141{,}012{,}904{,}202{,}240\text{ B}\approx\mathbf{128.25\ TB}$$

This is the **pointer-tree capacity**, about 128.25 TiB. However, the question also specifies **32-bit disk block addresses**: only $2^{32}$ distinct blocks can be numbered. With 2 KiB blocks, the entire addressable disk is at most $2^{32}\times2^{11}=2^{43}$ bytes, or **8 TiB**, including metadata. A real nonsparse file must be smaller still after allocating its indirect blocks and filesystem metadata. Thus **128.25 TiB is the usual tree-only exam calculation, not a realizable unique-data file size with 32-bit block numbers**. A sparse file could have a logical size near the tree limit while leaving most blocks unallocated, provided the file-offset/size fields are wide enough. Wider block numbers and enough storage would be needed to fully populate that tree with distinct data blocks.
