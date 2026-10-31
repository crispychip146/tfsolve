---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "It matters: putting new jobs at the back keeps RR fair (FIFO order); at the front, a steady stream of new jobs could starve the old ones."
sources: ["Tanenbaum MOS 4e, sec. 2.4.3 (round-robin)", "OSTEP ch. 7"]
changes:
  - "2026-10-31: Qualified response and waiting bounds for RR with arrivals."
---
**Yes, it makes a difference.**

- **New job at the back:** the new job waits for the jobs already in the queue. Every job gets a turn in arrival order, the queue stays FIFO, and no job is starved. This is the standard round-robin.
- **New job at the front:** the new job's *response time* improves (it runs at the next dispatch, unless later front insertions overtake it), but the older jobs are pushed back by every new arrival. With frequent arrivals, the jobs at the back of the queue can wait indefinitely (starvation), and RR degenerates into a LIFO-like policy that is unfair.

For a fixed set of $n$ runnable jobs, negligible dispatch cost and quantum $q$, the delay between turns is at most $(n-1)q$. With continuing arrivals, the bound must account for the actual queue length and scheduling overhead; it is not a universal fixed bound for an arbitrarily growing workload.
