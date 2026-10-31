---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "Idealized best 4A+C; spinning worst 4A+3C+T; blocking queue handoff can remove T. A queue-based spinlock does not."
sources: ["OSTEP ch. 28 (Locks), spin locks and queue locks"]
changes:
  - "2026-10-31: Clarified blocking queue-lock interpretation, preemption assumptions, and overhead limits."
---
Let thread $T_1$ run first and $T_2$ second. `do_something()` takes no time, so each thread costs one lock ($A$) and one unlock ($A$).

**i. Best case.** $T_1$ locks, unlocks and finishes inside its slice; one context switch; then $T_2$ does the same.

$$A + A + C + A + A = \mathbf{4A + C}$$

**ii. Worst case (at most three context switches, spinlock).**

1. $T_1$ acquires the lock: $A$.
2. The timer fires before $T_1$ unlocks: context switch to $T_2$: $C$.
3. $T_2$ tries to lock and **spins for its entire slice**, achieving nothing: $T$.
4. Context switch back to $T_1$: $C$.
5. $T_1$ unlocks: $A$ (it is done).
6. Context switch to $T_2$: $C$.
7. $T_2$ locks and unlocks: $A + A$.

$$A + C + T + C + A + C + 2A = \mathbf{4A + 3C + T}$$

**iii. Queue-based blocking lock.** When $T_2$ finds the lock held it puts itself on the lock's wait queue and sleeps (yields the CPU) instead of spinning, so the wasted slice $T$ disappears. The sequence is: $T_1$ lock ($A$), switch ($C$), $T_2$ tries the lock, enqueues and sleeps ($A$), switch back ($C$), $T_1$ unlock hands the lock to $T_2$ ($A$), switch ($C$), $T_2$ unlocks ($A$):

$$\mathbf{4A + 3C}$$

The worst case no longer depends on the time slice $T$; it is shorter by $T$ (the whole time $T_2$ would have spun).

**Assumptions:** the first thread can complete in one slice in the best case; preemption can occur while a thread holds the lock; a spinning thread uses its whole allotted slice; and lock hand-off costs no extra $A$. The question gives no separate cost for queue insertion, sleep or wakeup, so $4A+3C$ is the idealized blocking-lock answer under the same total bookkeeping budget. Add those costs if separately charged.

"Queue-based" does not automatically mean sleeping: MCS and ticket locks also queue waiters but **spin**. Such a queued spinlock still wastes a time slice when its owner is descheduled on one CPU; its worst case does not lose the $T$ term merely because there is a queue.
