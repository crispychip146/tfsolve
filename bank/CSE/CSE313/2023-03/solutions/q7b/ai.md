---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "Checksum batching retains TxE and removes only the internal journal wait; ordered metadata journaling must keep home data durable before commit."
sources: ["OSTEP ch. 42 (crash consistency: journaling)"]
changes:
  - "2026-10-31: Kept TxE and durable data-before-commit ordering in checksum metadata journaling."
---
Blocks to be updated: data blocks **8, 11, 12**; inode in block **3**; bitmap in block **2**. The journal occupies blocks **24-31**.

**i. Data journaling.** All the blocks are first written to the journal:

| Journal block | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 |
|:--|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| Contents | TxB | bitmap (2) | inode (3) | D8 | D11 | D12 | TxE | free |

Timeline:

1. **Journal write:** write TxB, bitmap, inode, D8, D11, D12 (24-29) and wait for them to complete.
2. **Journal commit:** write TxE (30) and wait: the transaction is committed.
3. **Checkpoint:** write bitmap (2), inode (3), D8, D11, D12 to their home locations.
4. **Free:** mark the transaction free in the journal superblock.

**ii. Metadata journaling.** Only the metadata (bitmap and inode) goes to the journal: TxB (24), bitmap (25), inode (26), TxE (27).

1. **Data write:** write D8, D11, D12 to their final locations.
2. **Journal write:** TxB, bitmap, inode (24-26). (Steps 1 and 2 may be issued together.)
3. Wait for *both* to complete, then **journal commit:** write TxE (27) and wait.
4. **Checkpoint:** write bitmap (2) and inode (3) to their final locations.
5. **Free** the transaction.

The data is written first so that the inode never points to garbage after a crash.

**iii. Metadata journaling with checksum optimisation.** The checksum detects an incomplete journal body, so the body and **TxE** can be submitted together. It does not cover data written outside the journal:

1. Write D8, D11 and D12 to their home locations and wait for durable completion.
2. Submit TxB (24), bitmap (25), inode (26) and **TxE (27)** as one checksum-protected journal batch; wait for durable completion. No separate body-before-TxE wait is required.
3. Checkpoint bitmap (2) and inode (3), wait for those writes to become durable, then free the journal space.

On recovery the checksum of the journalled blocks is recomputed; if it does not match, the transaction is discarded. This removes the separate wait within the journal. The data-before-commit dependency remains: otherwise valid-looking metadata could point to stale data. A cache acknowledgment alone does not establish durability; the storage interface must enforce the required flush/order guarantees.
