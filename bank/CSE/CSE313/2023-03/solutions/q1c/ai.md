---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "Atomic unsigned countdown ticket lock with acquire/release ordering; live tickets must remain unique across modular wraparound."
sources: ["OSTEP ch. 28 (ticket lock with fetch-and-add)"]
changes:
  - "2026-10-31: Made the ticket walkthrough match unsigned modular counters."
  - "2026-10-31: Explained atomic primitive adaptation and modular ticket uniqueness."
  - "2026-10-31: Made ticket-lock memory-order and counter assumptions explicit."
---
The code below is C-style pseudocode: adapt the supplied atomic instruction to an atomic **unsigned** counter, using `unsigned int FetchAndSubtract(_Atomic unsigned int *location)`. Include `<stdatomic.h>` for the other atomic operations. All accesses to the shared counters must be atomic; `volatile` alone does not prevent data races or provide memory ordering. The acquire load and release update also make critical-section writes visible to the next owner. Assume fewer outstanding tickets than the unsigned counter's range, so wraparound cannot duplicate a live ticket.

`FetchAndSubtract` is the mirror image of OSTEP's `FetchAndAdd`, so we can build a **ticket lock** whose counters count *down*. A thread takes a ticket by atomically decrementing `ticket`; it may enter when `turn` equals its ticket; unlocking just decrements `turn`.

```c
typedef struct __lock_t {
    _Atomic unsigned int ticket;   // next ticket to hand out
    _Atomic unsigned int turn;     // ticket currently allowed in
} lock_t;

void lock_init(lock_t *lock) {
    lock->ticket = 0;
    lock->turn   = 0;
}

void lock(lock_t *lock) {
    unsigned int myturn = FetchAndSubtract(&lock->ticket);  // returns old value, then ticket--
    while (atomic_load_explicit(&lock->turn, memory_order_acquire) != myturn)
        ;                                          // spin
}

void unlock(lock_t *lock) {
    atomic_fetch_sub_explicit(&lock->turn, 1, memory_order_release);                   // only the holder writes turn
}
```

**Why it works.**

- The first caller gets ticket 0, equal to `turn`, so it enters at once. The unsigned ticket counter decrements modulo its range and becomes `UINT_MAX`.
- The next caller gets `UINT_MAX` and spins until the holder's release decrements `turn` from 0 to `UINT_MAX`. Later tickets continue downward modulo the range. Each ticket is served in acquisition order, assuming holders eventually run and release the lock.
- **Mutual exclusion:** `FetchAndSubtract` is atomic, so no two threads get the same ticket, and only the thread whose ticket equals `turn` proceeds.
- **Fairness / no starvation:** tickets are served in FIFO order (a plain test-and-set spinlock gives no such guarantee).
- No thread ever has to undo a failed attempt. A simple "lock = 1, subtract, test for 1" attempt is *not* used: a failed subtract would leave the value wrong and we have no atomic "add back".
