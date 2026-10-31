---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "1082204160 bytes; central blocks 1056839 and 1056840 use triple-indirect indices 63,63,55 and 63,63,56."
sources: ["Bach, ch. 4 (algorithm bmap)"]
changes:
  - "2026-10-31: Identified both central blocks of an even-length file."
---
Block size 512 B, block number 4 bytes $\Rightarrow$ $512/4=128$ pointers per index block.

| Entries | Data blocks |
|:--|:-:|
| 16 direct | 16 |
| single indirect | $128$ |
| double indirect | $128^2=16{,}384$ |
| triple indirect | $128^3=2{,}097{,}152$ |
| **Total** | **2,113,680** |

$$2{,}113{,}680\times512\text{ B}=\mathbf{1{,}082{,}204{,}160\text{ bytes}}\approx1.01\text{ GiB}$$

**The middle data block.** The file has an **even** number of blocks, so it has two central blocks: zero-based indices **1,056,839** and **1,056,840**. Take the latter as the upper-middle block for the detailed path below.

- Direct blocks: $0\ldots15$; single indirect: $16\ldots143$; double indirect: $144\ldots16{,}527$. Block 1,056,840 is beyond these, so it is in the **triple-indirect** range.
- Offset in the triple range: $1{,}056{,}840-(16+128+16{,}384)=1{,}040{,}312$.
- First-level index (in the triple-indirect block): $\lfloor1{,}040{,}312/16{,}384\rfloor=\mathbf{63}$ (remainder $8{,}120$).
- Second-level index: $\lfloor8{,}120/128\rfloor=\mathbf{63}$ (remainder $\mathbf{56}$).
- Third-level index (in the single-indirect block): **56**.

**Path:** inode triple-indirect entry $\to$ block, entry **63** $\to$ block, entry **63** $\to$ block, entry **56** $\to$ the **data block** (the upper-middle block). The lower-middle block follows the same first two indices, **63, 63**, but ends at single-indirect index **55**.
