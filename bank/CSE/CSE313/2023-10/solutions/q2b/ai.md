---
author: ai
via: chat
status: unverified
summary: Yes. Making even philosophers take the left fork first and odd philosophers the right fork first prevents deadlock by breaking the circular-wait condition.
sources: [IPC slides 51-54, 'Tanenbaum, MOS 4e, sec. 2.5.1 and 6.6.4']
imported_from: tfsolve-questions/data/solutions/2021-22.json
changes:
  - "2026-10-31: Defined fork numbering and supplied a complete deadlock-free argument for the asymmetric protocol."
---
**Yes, the asymmetric protocol prevents deadlock** in the five-philosopher ring, assuming each fork is acquired atomically and a philosopher releases its forks after eating. It does not by itself guarantee starvation freedom.

Number forks so philosopher i has left fork i and right fork $(i+1)\bmod5$. Then philosopher 1 (odd) requests **fork 2 first**, and philosopher 2 (even) also requests **fork 2 first**. At most one can obtain it; the other waits while holding no fork.

Why does this exclude every deadlock, rather than just the familiar all-left example? A blocked philosopher that holds one fork can wait only for its other fork, held by a neighbour. In a ring of five, a cycle of such dependencies would have to go around the entire ring: two neighbours share only one fork, so they cannot form a two-process cycle with two different shared resources. But a whole-ring cycle requires every philosopher to hold one fork, impossible because philosophers 1 and 2 contend for the same **first** fork.

Thus circular wait cannot form. Someone holding a first fork can eventually acquire a second, eat and release resources, assuming it receives CPU time. Fair scheduling and fair fork queues are separate requirements if starvation must also be excluded.
