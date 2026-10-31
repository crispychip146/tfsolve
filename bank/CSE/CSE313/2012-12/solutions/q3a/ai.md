---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "Use a coherent cache mutex with threaded completion handling, or an IRQ-safe spinlock; an ordinary hard-interrupt spinlock alone is unsafe."
sources: ["Bach, ch. 3 (getblk, scenario 5); OSTEP ch. 28-30 (locks and condition variables)"]
changes:
  - "2026-10-31: Used one consistent cache lock and explained why an ordinary interrupt-context lock cannot replace interrupt masking."
---
**A lock can protect the buffer-cache structures, but an ordinary spinlock alone cannot safely replace interrupt masking when a hard interrupt handler takes the same lock.** If the interrupt preempts its owner on one CPU, the handler spins forever and the owner cannot resume. A sleeping mutex cannot be taken by a hard interrupt handler either.

There are two valid designs:

- Use an **interrupt-safe spinlock**, masking the relevant local interrupts while it is held. This handles other CPUs too, but does not eliminate interrupt masking entirely.
- If masking must be avoided, make the hard interrupt handler only record/queue completion using an interrupt-safe primitive. A **threaded handler** later performs cache-list updates under the same ordinary mutex as process code.

For the second design, use one `cache_lock` for the illustrated hash queues, busy flags and free list. Separate hash locks do not by themselves protect a shared global free list.

```text
getblk_scenario_5(device, block):
    lock(cache_lock)
    while true:
        buf = lookup(device, block)
        if buf exists and buf.busy:
            wait(buf.available, cache_lock)
            continue
        if buf exists:
            buf.busy = true
            remove_from_free_list(buf)
            unlock(cache_lock)
            return buf
        // Continue the other getblk scenarios under the same locking discipline.

brelse_from_process_or_threaded_handler(buf):
    lock(cache_lock)
    buf.busy = false
    insert_into_free_list(buf)
    broadcast(buf.available)
    broadcast(any_buffer_available)
    unlock(cache_lock)
```

The wait atomically registers the sleeper and releases the mutex, so a release cannot slip between the busy check and sleep. On wakeup the process redoes the lookup: another process may have acquired or reassigned the buffer. Assume buffer objects and their condition variables remain allocated while threads wait. List protection and sleep/wakeup atomicity are both necessary.
