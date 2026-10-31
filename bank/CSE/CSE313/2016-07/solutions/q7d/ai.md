---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "Semaphores have memory (a counter), so V() before P() is remembered; a condition variable has no memory, so a signal() with no waiter is lost and the order of wait() and signal() matters."
sources: ["Anderson and Dahlin, OSPP, ch. 5 (semaphores vs condition variables)"]
changes:
  - "2026-10-31: Explained retained semaphore notifications without claiming sequential P and V commute."
---
- A **semaphore** has an **integer counter** (its history). `V()` increments it even if nobody is waiting; a later `P()` finds the count $>0$, decrements it and does **not** block. Thus, when one thread posts and another waits, either relative arrival order can complete without losing the notification. They do **not** literally commute as arbitrary sequential statements: with an initial count of zero, `P(); V();` in the *same thread* blocks before it reaches V. Concurrent executions can also wake different waiters or have different intermediate states.
- A **condition variable** has **no memory**. `signal()` wakes a thread only if one is **already waiting**; if nobody is waiting the signal is **lost**. A `wait()` that comes *after* the `signal()` blocks and may sleep forever. So `signal(); wait()` is not equivalent to `wait(); signal()`: the order matters, and the waiter must check the condition (a shared variable, tested while holding the lock, with `while (!cond) wait()`).

That is why a condition variable is always used together with a lock and a state variable, whereas a semaphore stores the state itself.
