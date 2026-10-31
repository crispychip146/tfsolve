---
author: ai
via: chat
status: unverified
imported_from: cse313_mem_fs_solved.tex
changes:
  - "2026-10-31: Removed unsupported claim that append-only segment writes automatically make unlink atomic."
  - "2026-09-30 (import): split from the combined (i)-(iii) solution."
---
Recovery begins with the latest complete checkpoint and scans later log records to reconstruct newer state. The important question is whether **all logically dependent parts of the delete** are recoverable: the parent directory update, the file's link-count update, and the relevant inode-map changes.

- If no complete deletion operation became durable, recovery retains the earlier consistent state. A torn or incomplete log tail must not be treated as a committed deletion.
- If a complete deletion operation is durable beyond the checkpoint, roll-forward applies it, so the name remains removed.
- If the crash leaves only part of the operation, the recovery protocol must reconcile those related updates, using operation records, transaction boundaries, or the filesystem's directory-recovery procedure. Merely appending blocks does not make a multi-block or multi-segment operation atomic.

A segment checksum can help detect an incomplete write, but cannot by itself prove that every part of an `rm` operation was included. Assuming a correct atomic-update/recovery protocol, the recovered namespace reflects either the old state or the completed delete. Open descriptors and other hard links still determine when the underlying file can be reclaimed.
