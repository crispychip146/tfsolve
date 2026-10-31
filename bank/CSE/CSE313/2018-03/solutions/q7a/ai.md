---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "Two-phase locking: a growing phase in which all locks are acquired and a shrinking phase in which they are released; no lock is acquired after the first release."
sources: ["Tanenbaum MOS 4e, sec. 6.8.2 (two-phase locking)"]
changes:
  - "2026-10-31: Distinguished the course deadlock-avoidance retry variant from standard database two-phase locking."
---
**Two-phase locking** divides a transaction's lock use into two phases:

1. **Growing:** it may acquire locks but does not release any.
2. **Shrinking:** after its first release, it may release further locks but cannot acquire a new one.

This rule gives conflict-serializable schedules when transactions follow the usual read/write locking rules. **Ordinary database 2PL can still deadlock**: one transaction can hold A and wait for B while another holds B and waits for A.

In the **deadlock-prevention variant discussed in OS texts**, a process first tries to acquire *all* the locks it needs before doing the protected work. If any acquisition fails, it releases the locks acquired so far and retries later; once all are held, it performs the work and releases them. This requires nonblocking acquisition attempts, rather than sleeping while retaining partial ownership. Such retries prevent resource deadlock but may cause livelock or starvation without backoff/fairness.

The names are sometimes used together in introductory OS discussions. Keep the guarantee attached to its rule: the basic acquire-then-release phase rule provides serializability; the **release-and-retry on failed acquisition** rule is the extra mechanism that removes hold-and-wait. Do not conclude that every implementation called 2PL is deadlock-free.
