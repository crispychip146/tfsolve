---
author: ai
via: chat
status: unverified
imported_from: cse313_mem_fs_solved.tex
changes:
  - "2026-10-31: Added safe buffer ownership, all requested driver pseudocodes, and the missing C/output assumptions."
---
**(i) The driver sees writes, not the loop variable n.** For n=10, one call produces 10 bytes, `0123456789`. For n=1000000, it produces 5,888,890 decimal-digit bytes through one million tiny system calls. A large n does not make any individual `write()` large. Driver buffering reduces device overhead; batching in the application/library is also needed to reduce the syscall count.

**(a) Frequent short calls.** Use a bounded kernel queue, coalesce small writes, and flush on a byte threshold or short latency timer. The timer prevents a short tail from waiting forever.

```text
driver_write(bytes):
    lock(queue)
    wait_for_queue_space()
    copy_into_queue(bytes)
    arm_flush_timer_if_needed()
    if queued_bytes >= threshold: submit_ready_batch()
    unlock(queue)
```

**(b) Sustained large output.** Still coalesce the tiny writes. Transfer batches by DMA, keep multiple buffers/descriptors in flight, and use completion interrupts with coalescing or bounded polling where measured completion rates justify it.

```text
submit_ready_batch():
    batch = detach_filled_buffer_from_queue()
    mark_in_flight(batch)
    submit_DMA(batch)       // batch storage remains pinned and unchanged

on_completion(batch):
    record_status(batch)
    reclaim_buffer(batch)  // only now may its storage be reused
    wake_waiters_for_space()
    submit_more_if_available()
```

Calling `start_dma(ring_buffer); ring_buffer.clear()` is unsafe if clearing allows writers to overwrite bytes still being read by DMA. Ownership passes to the device until completion. A driver cannot assume n alone determines whether polling is better: device latency and the number of outstanding requests also matter.

**(c) Mixed small and large traffic.** Use the same bounded queue and timer, with adaptive completion handling:

```text
if sustained_backlog_and_frequent_completions:
    process_completions_with_a_bounded_poll_budget()
else:
    enable_completion_interrupts_and_sleep_when_idle()
flush_a_short_pending_batch_when_its_timer_expires()
```

This balances short-write latency, high-load throughput and CPU cost. Queue limits provide backpressure; error status must be reported rather than silently dropping bytes. These are illustrative driver designs, not assumptions about a particular unspecified device.

**(ii) Two open calls versus dup.** The printed snippets omit required arguments: take both `open` calls as successful writable opens **without O_APPEND or truncation**, and take each `write_fd(fd)` as the intended `write_fd(fd, n)` for the same n. Assume complete writes and no concurrent modification. As printed, the snippets are incomplete C.

| Structure | Two independent opens | Open followed by dup |
|:--|:--|:--|
| Descriptor fd1 | points to open-file entry A | points to entry A |
| Descriptor fd2 | points to independent entry B | also points to entry A |
| File offsets | A=0 and B=0 initially | one shared offset, initially 0 |
| Inode | A and B refer to the same inode | A refers to that inode |

With independent opens, the first call writes its digit sequence and advances A's offset. The second starts at B's still-zero offset and **overwrites the same prefix**. With dup, the first call advances the shared offset and the second **continues immediately after it**, giving the two sequences concatenated. For n=10 on an initially empty file, results are respectively `0123456789` and `01234567890123456789`. An existing suffix can remain in the first case because no truncation was assumed. O_APPEND would change the independent-open result.

Production code must also handle partial writes/errors and provide a large enough conversion buffer. For the specified range up to n=1000000, `buf[10]` holds every printed number and its terminator; it is not enough for all possible int values.
