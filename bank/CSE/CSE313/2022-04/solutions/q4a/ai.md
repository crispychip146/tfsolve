---
author: ai
via: chat
status: unverified
summary: 'Compute-bound: long CPU bursts, rare I/O (timeline mostly CPU). I/O-bound: short CPU bursts with frequent long I/O waits (timeline mostly waiting).'
sources: [Scheduling slides 5-7, 'Tanenbaum, MOS 4e, sec. 2.4.1 (Fig 2-39)']
imported_from: tfsolve-questions/data/solutions/2019-20.json
changes:
  - "2026-10-31: Added actual CPU/I/O timelines and corrected the cause of an I/O-bound trend."
---
- Compute-bound (CPU-bound): a long CPU burst then a short I/O, repeatedly, the timeline is mostly CPU activity. Limited by CPU speed.
- I/O-bound: a short CPU burst then a long I/O wait, repeatedly, the timeline is mostly device waiting. Limited by device speed.

![Illustrative CPU-bound and I/O-bound burst timelines](figures/bursts.png)

The durations are illustrative, not values given in the paper. CPU-bound means the CPU is the main bottleneck; it does not require every I/O wait to be short. If CPU execution becomes faster while device latency stays similar, a workload can become relatively more I/O-bound. Simply giving a process more memory or more CPU time does not by itself establish that change.
