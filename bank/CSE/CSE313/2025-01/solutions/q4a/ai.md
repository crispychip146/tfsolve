---
author: ai
via: chat
status: unverified
summary: "CPU starvation usually leaves a process Ready; resource starvation can leave it Blocked. Livelock executes retries without progress."
sources: [Process/Thread slides 11-14, 'Tanenbaum, MOS 4e, sec. 2.1.5 and 6.7']
imported_from: tfsolve-questions/data/solutions/2022-23.json
changes:
  - "2026-10-31: Added the requested life-cycle figure and distinguished CPU starvation from resource starvation."
---
![Process life cycle with ready, running and blocked states](figures/five.png)

States and transitions: New $\to$ Ready (admitted); Ready $\to$ Running (dispatch); Running $\to$ Ready (time-out / preempt); Running $\to$ Blocked (wait for I/O or event); Blocked $\to$ Ready (event done); Running $\to$ Terminated (exit).

- (i) Starvation: CPU starvation leaves the process **Ready**, repeatedly passed over by the scheduler. Resource starvation can instead leave a process **Blocked** waiting for a resource that is repeatedly granted to others; the question needs this distinction.
- (ii) Livelock: the process continues executing or being scheduled (Running when on the CPU, Ready when preempted), but keeps retrying without making useful progress, e.g. two processes repeatedly backing off and retrying so neither proceeds.
