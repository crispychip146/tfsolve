---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "After deleting A and C, blocks 3-6 and 8 are free: E (5 MB) cannot be created with contiguous allocation (without compaction), fails with pointers inside blocks (needs 6 blocks), and works with a FAT (blocks 3,4,5,6,8)."
sources: ["Tanenbaum MOS 4e, sec. 4.3.2 (fragmentation)"]
changes:
  - "2026-10-31: Kept linked-list allocation consistent with its pointer overhead in the preceding part."
---
After removing **A** (blocks 3-6) and **C** (block 8), the free data blocks are **3, 4, 5, 6 and 8** (5 blocks in total); B (block 7) and D (block 9) remain. **E needs 5 MB $=5$ blocks.**

![State after deleting A and C and creating E](figures/ssd2.png)

**(i) Contiguous allocation.** The free space is split into holes of 4 blocks (3-6) and 1 block (8): there is **no run of 5 contiguous blocks**, although 5 blocks are free ($\Rightarrow$ **external fragmentation**). E **cannot be created** unless the OS first **compacts** the disk (moves B and D to blocks 3-4, giving blocks 5-9 free, which costs copying 2 MB) and then puts E in 5-9.

**(ii) Linked-list allocation.** Follow the pointer-overhead interpretation from 7(a): A occupies 3-6, B needs blocks 7-8, C occupies 9, and D could not be created. Deleting A and C therefore frees blocks 3-6 and 9, again five blocks. Each loses a few bytes to its link, so five blocks hold less than 5 MiB and E needs **six** blocks. **E cannot be created.** If an exercise ignores link storage, all four original files fit and E can instead occupy the five freed blocks noncontiguously; that is a different stated assumption.

**(iii) Linked list with a FAT.** The links are in the FAT, so each of the five free blocks holds a full 1 MB: **E is stored in blocks 3$\to$4$\to$5$\to$6$\to$8**, with no compaction and no wasted space; the FAT entries are updated (3$\to$4, 4$\to$5, 5$\to$6, 6$\to$8, 8$\to$end).
