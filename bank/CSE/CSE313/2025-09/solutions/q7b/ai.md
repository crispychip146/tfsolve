---
author: ai
via: chat
status: unverified
imported_from: cse313_mem_fs_solved.tex
changes:
  - "2026-10-31: Separated ordered/writeback modes, existing-block overwrites, and committed from incomplete data journals."
---
A journal makes a group of persistent changes recoverable. **Commit** means the entire required transaction has reached durable storage; **checkpoint** means copying its committed contents to their ordinary home locations. Distinguish structural metadata consistency from whether the latest user bytes survived.

**(i) Data D reaches home before the metadata commit.** Recovery does not replay the incomplete metadata transaction. For a newly allocated block, the old committed inode does not reference D and the old bitmap still treats it as free: the unreferenced bytes do not by themselves create an allocated-block leak. If D instead overwrites an *existing* file block, old metadata can still reference it and the new bytes may be visible. Metadata journaling does not roll back arbitrary data overwrites.

**(ii) Metadata commit reaches disk before D.** This is possible in **writeback** metadata journaling, or if ordered-mode rules are broken. Recovery can replay an inode that references D even though D still contains old, unrelated bytes. Allocation metadata can be structurally consistent while the file exposes stale data. Correct **ordered** metadata journaling prevents this newly-allocated-block case by making D durable before committing metadata that exposes it.

**(iii) Data is journaled but not yet at home.** If **data and metadata belong to a complete, durable committed transaction**, recovery replays them and finishes the checkpoint, giving their consistent new state. If only the data block reached the journal and the transaction never committed, recovery ignores that transaction. The question's phrase "data is journaled" alone does not establish commitment. Committed transactions remain in the journal until their home writes are durable.

**(iv) Comparison.** Metadata-only journaling usually does less write I/O because regular-file data is written to home once. Full data journaling normally writes data twice, to journal and then home, but protects the committed data and metadata together. Performance also depends on batching and access patterns.

Full data journaling gives the stronger transactional data-recovery guarantee. Ordered metadata journaling also prevents exposure of *unwritten newly allocated blocks* when its ordering is correctly enforced. Neither mode promises that every uncommitted write survives; applications need the appropriate synchronization operation, such as `fsync()`, when durability is required. Thus "only data journaling can ever prevent garbage" is too broad, and metadata writeback is the unsafe mode illustrated by (ii).
