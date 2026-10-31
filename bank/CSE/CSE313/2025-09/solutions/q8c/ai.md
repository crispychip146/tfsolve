---
author: ai
via: chat
status: unverified
imported_from: cse313_mem_fs_solved.tex
changes:
  - "2026-10-31: Explained validation of checkpoint copies and limits of segment-level recovery."
---
**Two crash scenarios during a disk write in LFS:**

1.  **Crash while writing out a segment itself** -- the segment (data + inode(s) + segment summary) is only partially written to disk (a torn write); some of its blocks made it to disk, others did not.

2.  **Crash while updating the checkpoint region (CR)** -- the fixed-location checkpoint region, which periodically records where the most recent valid part of the log/imap is, could itself become corrupted/incomplete if the crash happens exactly during its update.

LFS defends against (1) by including a checksum in each segment's summary block: at recovery time, if the checksum does not match the blocks actually present, that (partial) segment is discarded, and the log is treated as ending at the last fully-valid, checksum-verified segment. It defends against (2) by keeping **two** checkpoint regions and writing to them alternately (with a protocol that validates each complete copy, for example matching beginning/end timestamps), so under the assumed single-crash storage model one complete checkpoint remains usable while the other is updated. A lone new timestamp at the start of a torn checkpoint must not make it look valid.

**Need for roll forward.** Checkpoints are only written periodically (writing one after every single segment would be far too expensive), so there is normally a gap between the last checkpoint and the actual, later segments that were fully and successfully written to the log *after* that checkpoint but before the crash. If recovery relied on the checkpoint region alone, all of that later (perfectly valid, already-on-disk) work would be lost. **Roll forward** fixes this: after restoring state from the last valid checkpoint, LFS scans *forward* through the log from that point, reading and validating (via checksums) each subsequent segment's summary block, and replays every complete, valid segment it finds -- bringing the file system fully up to date with everything that was actually, durably written before the crash, rather than needlessly discarding it. Recovery must also enforce the consistency rules for multi-block directory operations; a valid segment checksum alone does not prove a multi-segment rename/unlink operation is complete.
