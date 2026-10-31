---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "Ignore, detect/recover, avoid, or prevent. The textbook ostrich answer concerns general application deadlocks, not all kernel subsystems."
sources: ["Tanenbaum MOS 4e, sec. 6.1-6.7 (deadlock strategies, ostrich algorithm)"]
changes:
  - "2026-10-31: Qualified the textbook ostrich-policy comparison instead of asserting all OS subsystems ignore deadlock."
---
**The four strategies** (Tanenbaum):

1. **Ignore the problem** (the ostrich algorithm).
2. **Detection and recovery:** let deadlocks occur, detect them and recover.
3. **Dynamic avoidance** by careful resource allocation (Banker's algorithm, safe states).
4. **Prevention:** structurally negate one of the four necessary conditions.

**Textbook answer: the ostrich approach is common for general application resource deadlocks.** An OS may leave these for applications/users to resolve rather than run a universal detection or avoidance algorithm. This does not mean UNIX, Linux or Windows ignore every deadlock risk: kernel lock ordering, database lock detection and subsystem-specific recovery are other strategies. *Reason for the textbook trade-off:*

- When application deadlocks are sufficiently infrequent for a particular workload, the overhead/restrictions of a universal prevention or avoidance scheme may outweigh its benefit.
- Prevention puts **restrictions on users** (e.g. request everything at once, resource ordering) and lowers resource utilisation; avoidance needs the maximum needs in advance and is expensive; detection costs overhead and recovery is crude.
- There is a trade-off between **convenience and correctness**: most users prefer a rare reboot to constant restrictions.
