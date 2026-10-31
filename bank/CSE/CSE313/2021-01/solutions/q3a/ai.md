---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "The Coffman conditions are necessary; with multiple instances a cycle can be resolved by an outside holder releasing another instance."
sources: ["Tanenbaum MOS 4e, sec. 6.2 (introduction to deadlocks, Coffman conditions)"]
changes:
  - "2026-10-31: Replaced a counterexample missing circular wait with a multiple-instance resource example."
---
The four necessary conditions are:

1. **Mutual exclusion:** a resource instance cannot be used by two processes simultaneously.
2. **Hold and wait:** a process retains some resources while requesting others.
3. **No preemption:** the system cannot forcibly take those resources away.
4. **Circular wait:** processes form a cycle, each waiting for a resource held by the next.

To see why a cycle is not sufficient when resource types have **multiple instances**, let type $R$ have two instances and type $S$ have one:

| Process | Holds | Requests |
|:--|:--|:--|
| P1 | one R | one S |
| P2 | S | one R |
| P3 | the other R | nothing; can finish |

There is a cycle $P1\to S\to P2\to R\to P1$. Mutual exclusion, hold-and-wait and non-preemption also hold. Nevertheless, **the system is not deadlocked**: P3 finishes and releases its R; P2 obtains that instance, finishes and releases S; P1 then finishes.

The cycle describes resource *types*, so an extra instance released by a process outside it can break the wait. With exactly one instance of each resource type, a cycle of actual outstanding requests is sufficient for deadlock. An example in which the processes merely run one after another does not demonstrate all four conditions simultaneously, because it contains no circular wait.
