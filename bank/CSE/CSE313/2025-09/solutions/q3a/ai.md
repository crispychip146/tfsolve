---
author: ai
via: chat
status: unverified
summary: A safe state has an ordering letting every process reach its maximum and finish; an unsafe state has none (but is not yet a deadlock). Banker's grants a request only if the resulting state is still safe.
sources: [Deadlocks slides 25-31, 'Tanenbaum, MOS 4e, sec. 6.5']
imported_from: tfsolve-questions/data/solutions/2023-24.json
changes:
  - "2026-10-31: Added request validation and distinguished simulated Work from real Available."
---
- Safe state: there is an ordering $P_{i_1},\dots,P_{i_n}$ such that each $P_{i_k}$'s remaining need can be met by the currently available resources plus those held by $P_{i_1},\dots,P_{i_{k-1}}$.
- Unsafe state: no such ordering exists. Unsafe $\ne$ deadlock, the processes might still finish if they do not all claim their maxima, but the system can no longer guarantee it.

Banker's algorithm keeps Allocation, declared Max and Available; the additional amount each process might still need is **Need = Max - Allocation**. For a request from process i:

1. Require Request to be nonnegative and no greater than Need[i]; a larger request exceeds its declared claim.
2. If Request exceeds Available, make the process wait.
3. Otherwise tentatively subtract Request from Available, add it to Allocation[i], and subtract it from Need[i].
4. Test the tentative state using a **copy**, Work = Available. Find an unfinished process with Need <= Work, simulate its completion by adding its original Allocation to Work, and repeat. Do not change the real Available while merely simulating completions.
5. If every process can finish, keep the grant. Otherwise restore all tentative changes and make the requester wait.

The safety proof follows the sequence constructed in step 4: the first process can obtain its remaining need, finish and release resources; each next process can then do the same. If no candidate exists, no safe sequence starts from the simulated remaining state. This means **unsafe**, not necessarily already deadlocked.
