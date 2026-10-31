---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "Weighted CPU budgets 50:30:20; priority within system jobs, RR for interactive jobs, SJF for batch jobs."
sources: ["Tanenbaum MOS 4e, sec. 2.4.1 (scheduling goals, real-time systems)"]
changes:
  - "2026-10-31: Replaced an unrelated real-time answer with the requested weighted three-class scheduler."
---
We need two decisions: **which class receives CPU time**, and **which process runs within that class**. A single priority queue cannot guarantee the requested shares: a permanently ready system process could otherwise consume the entire CPU.

Use three ready queues and give them CPU-time budgets in the ratio **50:30:20**. For an illustrative accounting frame of 100 ms, system jobs receive 50 ms, interactive jobs 30 ms and batch jobs 20 ms. This is a design example, not a time quantum specified by the paper. Charge actual execution time to the class's budget; do not count dispatches, since jobs may block early.

| Class | Share while all classes are runnable | Selection within the class |
|:--|:-:|:--|
| System | 50% | Highest predefined importance first; RR for ties |
| Interactive | 30% | Round robin with equal quanta |
| Batch | 20% | Shortest job first, since bursts are known and jobs arrive together |

```text
every accounting frame:
    budget = {system: 50, interactive: 30, batch: 20}
    while a class has both ready work and unused budget:
        c = next eligible class in the cyclic class order
        p = choose_using_class_policy(c)
        used = run(p, bounded_by_class_budget_and_time_slice)
        budget[c] -= used
```

The budget caps prevent a busy high-priority class from taking another class's reserved share. Within the system class, importance determines the order. Interactive RR treats equal-priority users fairly. Within the batch class, resuming the shortest selected job before starting the next preserves SJF's completion order even when another class's turn interrupts it.

**Assumptions and trade-offs.** Ignore switching overhead in the 50:30:20 accounting. If a class becomes empty, lend its unused CPU time to runnable classes so the CPU does not idle; the exact percentages apply when all classes are backlogged. A shorter accounting frame improves responsiveness but adds switching overhead. Strict priority within system jobs can starve low-importance system jobs; aging can address that if the required importance policy permits it.
