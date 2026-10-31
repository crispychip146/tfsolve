---
author: ai
via: chat
status: unverified
imported_from: cse313_mem_fs_solved.tex
changes:
  - "2026-10-31: Protected dirty-page write-back against concurrent writes and avoided spinning when all candidates are pending."
summary: "Clock prefers unreferenced clean frames, writes dirty pages back asynchronously, preserves redirty state and waits when all candidates need I/O."
---
Ordinary Clock approximates recent use: the MMU sets a page's **reference bit R** on access, and the replacement hand clears it to give the page a second chance. A **dirty page** has changed since its backing copy was saved, so reusing its frame immediately would lose data.

**(i) Bookkeeping.** Keep R and the **dirty/modified bit M** in the PTE or equivalent per-page state. Also keep software flags for write-back in progress and whether the page was written again during that write-back. The latter flags normally belong to the kernel's page/frame descriptor; they need not consume hardware PTE bits.

**(ii) Hardware and OS support.** The MMU should record references and writes, or the OS must emulate them with protection faults. The OS must clear/sample those bits using the architecture's TLB synchronization rules. The storage subsystem needs asynchronous write-back; page locking, write protection, or a stable I/O snapshot must make the bytes being written consistent.

**(iii) Replacement steps.**

1. Scan eligible resident frames in a circular order. Skip pinned or otherwise unevictable frames.
2. If R is 1, clear R and advance: the page gets another chance.
3. If R is 0 and the page is clean with no write-back pending, invalidate its mappings/TLB entries safely and reuse the frame.
4. If R is 0 and the page is dirty, start write-back if none is pending, then continue scanning. Do not overwrite its frame while I/O still reads it.
5. After a full scan with no usable victim, wait for write-back or another reclaim event, rather than spinning indefinitely.

**The concurrent-write case matters.** Suppose the OS starts saving version A, the process then writes version B, and the I/O of A completes. Simply clearing M at completion would falsely label B clean. A correct implementation clears/records the dirty state when taking the stable write-back version, records later writes separately, and marks the page clean on completion **only if it has not been dirtied again**. Otherwise another write-back is required. Recheck R and mapping state before eviction too.

The policy prefers unreferenced clean victims and starts expensive writes early so that I/O can overlap other execution. It improves the timing of write-back; it does not remove the need to preserve every dirty version still required by a live mapping.
