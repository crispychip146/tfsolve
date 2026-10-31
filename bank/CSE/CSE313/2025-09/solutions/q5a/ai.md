---
author: ai
via: chat
status: unverified
imported_from: cse313_mem_fs_solved.tex
changes:
  - "2026-10-31: Included unused root entries and distinguished packed table sizes from page-sized allocations."
summary: "Single-level 4 MiB; packed three-level tables 6 KiB with 1936 B of unused entries; one frame per table instead consumes 84 KiB."
---
32-bit VA, 4KB pages $\Rightarrow$ 20 bits of VPN, 12 bits offset. 3-level split: 8 / 6 / 6 bits (level-1 / level-2 / level-3), matching the 20-bit VPN ($8+6+6=20$). PTE/PDE size $=4$B.

**(i) Single-level page table.** It must have one entry for *every* possible VPN, whether used or not: $2^{20}$ entries $\times 4$B $= 4{,}194{,}304$B $= \mathbf{4\,MB}$.

**(ii) 3-level page table.** Only branches of the tree that are actually used get allocated.

- *Level-1 table*: always fully allocated: $2^{8}=256$ entries $\times4$B$=1024$B $=1$KB.

- Code (VPN 0--3) and Heap (VPN 1024--2047) both fall under level-1 index $0$ (since $2047\gg12=0$). Stack (VPN $2^{20}-4 .. 2^{20}-1$) falls under level-1 index $255$ (the top index). So only **2 level-1 entries** are valid $\Rightarrow$ 2 level-2 tables, each $2^{6}\times4$B$=256$B $\Rightarrow 2\times256=512$B.

- Under level-1 index 0: code uses level-2 index 0 (1 entry), heap (1024 pages / 64 = exactly 16 full level-3 tables) uses level-2 indices 16--31 (16 entries) $\Rightarrow 17$ valid level-2 entries $\Rightarrow$ 17 level-3 tables.

- Under level-1 index 255: stack uses a single level-2 entry (index 63) $\Rightarrow$ 1 level-3 table.

- Total level-3 tables $=17+1=18$, each $256$B $\Rightarrow 18\times256=4608$B.

**Total** $=1024\text{B (L1)}+512\text{B (L2)}+4608\text{B (L3)}=6144\text{B}=\mathbf{6\,KB}$.

**Comparison**: $4$MB (single-level) vs. $6$KB (3-level) -- a $\sim$ 683$\times$ reduction, because the multi-level table only allocates space for the parts of the (mostly unused) 20-bit VPN space that the process actually touches.

**(iii) Fragmentation / wastage.** Every allocated table node (level-2 or level-3) is a *fixed-size* array of 64 entries, even when only a few of those entries are actually valid:

- Code's level-3 table: only 4 of 64 entries used $\to$ 60 entries ($240$B) wasted.

- Stack's level-3 table: only 4 of 64 entries used $\to$ 240B wasted.

- Level-2 table under L1=0: 17 of 64 entries used $\to$ 47 entries ($188$B) wasted.

- Level-2 table under L1=255: 1 of 64 entries used $\to$ 63 entries ($252$B) wasted.

- Heap's 16 level-3 tables are exactly full (1024 pages / 64 per table) $\to$ no waste there.

- The root also has 254 unused entries: $254\times4=1016$B. Total unused-entry storage is therefore $480+188+252+1016=\mathbf{1936\text{ B}}$.

The **6 KiB** result assumes the allocator packs each table at its actual size. If every table must occupy a separate 4 KiB physical frame, there are $1+2+18=21$ table frames, consuming **84 KiB**. The unused frame tails alone waste $84-6=78$ KiB, in addition to unused entries inside the table arrays. The question does not specify the allocation granularity, so these assumptions must be distinguished.

This is internal allocation waste: space reserved inside table nodes or table frames. Noncontiguous physical frames can hold different table nodes, so no large contiguous physical region is required. A direct remedy is to pack 256B and 1024B tables into physical pages using a suitable sub-page allocator, if the hardware permits their alignment. Alternatively, a split such as 10/10/12 makes each table exactly one page, but changes this question's specified layout. Hashed or inverted tables are other designs, with different lookup costs; an inverted table scales with physical frames, rather than the process's virtual-page count.
