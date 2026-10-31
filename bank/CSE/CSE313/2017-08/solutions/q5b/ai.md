---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "Enumeration can scale with CPU cores using private wide partial sums; one core gets no speedup. If enumeration is unnecessary, use the closed-form sum."
sources: ["Tanenbaum MOS 4e, sec. 2.2 (threads); Amdahl's law"]
changes:
  - "2026-10-31: Stated integer range/overflow assumptions and the closed-form alternative to exhaustive summation."
---
**Interpretation.** If the task is to enumerate and add all $2^{64}$ bit patterns interpreted as **unsigned** integers, there are about $1.8\times10^{19}$ terms. Use an accumulator wide enough for the exact result; a 64-bit sum would overflow.

**Single-threaded:** one loop does all additions; at $10^9$ additions per second it takes about $1.8\times10^{10}$ s, i.e. **about 585 years**.

**Multithreaded design:** split the range into $p$ equal sub-ranges, one thread per sub-range, each computing a **private partial sum**; at the end the partial sums are added (the main thread does `join` and adds $p$ numbers). Because the partial sums are private there is **no need for locking** inside the loops.

**Effect on the time.**

- On a **multiprocessor/multicore** machine with $p$ cores the threads really run in parallel, so the time is about $T/p$ plus the tiny cost of creating the threads and combining the results: a near-linear **speed-up** (about $p\times$, limited by $p$ cores and memory/overhead); the serial part is negligible (Amdahl's law).
- On a **single CPU** the threads take turns, so there is **no speed-up** (the total work is the same; the context switches add a little overhead, making it slightly slower).
- If the threads updated one shared sum, locking would serialise them and destroy the gain; hence private partial sums.

**Algorithmic alternative.** If enumeration is not required, pair the first and last integers. The unsigned range $0$ through $2^{64}-1$ sums to $2^{63}(2^{64}-1)$, computable directly with 128-bit or arbitrary-precision arithmetic. For the signed range $-2^{63}$ through $2^{63}-1$, pairing opposites leaves the sum $-2^{63}$. These formulas avoid the huge loop altogether; multithreading only helps the explicitly required enumeration approach.
