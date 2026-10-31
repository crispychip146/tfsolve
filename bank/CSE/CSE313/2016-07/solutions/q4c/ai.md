---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "Write-through propagates stores toward memory (possibly buffered); write-back marks dirty and writes on eviction, explicit flush or coherence demand."
sources: ["Anderson and Dahlin, OSPP, ch. 9 (caching: write-through and write-back)", "Tanenbaum MOS 4e, sec. 1.3 (memory hierarchy, caching)"]
changes:
  - "2026-10-31: Included explicit flush/coherence write-back and clarified buffered write-through."
---
![Write-through and write-back caches](figures/writepol.png)

**Write-through.** Every write goes to the **cache and to main memory** at the same time (often through a write buffer). *Pros:* writes are propagated toward memory immediately; a write buffer may still contain pending stores, so memory/DMA visibility requires the specified ordering. *Cons:* every write costs a memory access, so it is slower and uses more bus bandwidth.

**Write-back (copy-back).** A write updates **only the cache** and sets the block's **dirty bit**; main memory is updated when a dirty block is evicted, explicitly flushed, or written back as required by the coherence protocol. *Pros:* repeated writes to the same block cost only one memory write, so it is much faster and uses less bandwidth. *Cons:* memory can be stale, evicting a dirty block costs a write, and coherence (other CPUs, DMA) needs more care.

On a write **miss** the policy is either *write-allocate* (bring the block into the cache first, usual with write-back) or *no-write-allocate* (write directly to memory, usual with write-through).
