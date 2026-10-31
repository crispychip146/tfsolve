---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "Detection leaves P2 unmarked; its demand also exceeds the total number of R3 instances, so the printed request is impossible."
sources: ["Tanenbaum MOS 4e, sec. 6.4.2 (deadlock detection with multiple resources of each type)"]
changes:
  - "2026-10-31: Flagged an impossible request while retaining the detection algorithm result."
---
Matrices (processes P1-P4, resources R1-R4): current allocation $C$, request $R$ and available vector $A=(3,2,2,1)$.

$$C=\begin{pmatrix}1&3&0&1\\4&1&2&2\\1&2&0&0\\2&0&0&1\end{pmatrix}$$

$$R=\begin{pmatrix}1&4&2&1\\0&2&3&3\\1&1&1&2\\3&1&1&1\end{pmatrix}$$

**Algorithm:** repeatedly find an unmarked process whose request row is $\le A$; add its allocation row to $A$ and mark it; unmarked processes at the end are deadlocked.

| Step | Process | Request row $\le A$? | New $A$ |
|:-:|:-:|:--|:-:|
| 1 | P1 $(1,4,2,1)$ | $4>2$: no | |
| 2 | P2 $(0,2,3,3)$ | $3>2$: no | |
| 3 | P3 $(1,1,1,2)$ | $2>1$: no | |
| 4 | P4 $(3,1,1,1)$ | $(3,1,1,1)\le(3,2,2,1)$: **yes**, mark P4 | $A+(2,0,0,1)=(5,2,2,2)$ |
| 5 | P3 $(1,1,1,2)$ | yes, mark P3 | $A+(1,2,0,0)=(6,4,2,2)$ |
| 6 | P1 $(1,4,2,1)$ | yes, mark P1 | $A+(1,3,0,1)=(7,7,2,3)$ |
| 7 | P2 $(0,2,3,3)$ | $3>2$ (R3): no | |

No further process can proceed and **P2 is unmarked**: **the system is deadlocked, and the deadlocked process is P2.** P2 needs 3 more units of R3, but only 2 units of R3 are free and nobody else holds any R3 that could be released (R3 is held only by P2 itself and P1/P3/P4 hold none), so its request can never be satisfied.

**Data-consistency caution.** The total number of R3 instances is $2$ held by P2 plus $2$ initially available, or **4**. P2 holds 2 and requests **3 additional** units, implying a total demand of 5, greater than the system total. The matrix algorithm leaves P2 unmarked, but this is an **impossible-demand input**, rather than an ordinary cycle between processes. If R was intended as a maximum-claim matrix instead of the stated current-request matrix, first subtract C and solve that different problem; do not silently change its meaning.
