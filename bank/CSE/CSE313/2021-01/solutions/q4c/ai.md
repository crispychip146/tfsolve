---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "Banker needs declared maxima and accurate resource accounting; global resource ordering prevents circular wait."
sources: ["Tanenbaum MOS 4e, sec. 6.5.4 and sec. 6.6.4 (attacking circular wait)"]
changes:
  - "2026-10-31: Qualified dynamic admission and safety-check cost rather than treating implementation assumptions as impossibilities."
---
**Why Banker's algorithm is not useful in practice.**

- It needs every process to **declare its maximum resource needs in advance**, which is generally unknown.
- It needs an accurate model of admitted processes, maximum claims and available resources. New processes can be admitted with a safety check, but dynamic admission and resource failures complicate maintaining that model.
- It must run on **every request** and costs $O(m\,n^2)$ for $n$ processes and $m$ resource types, which can be significant for large systems or high request rates; it is not inherently too slow for every workload.
- It assumes that processes are independent (no synchronisation constraints) and is very conservative, so it limits concurrency.

**Eliminating circular wait (prevention).** Impose a **global numbering of all resources** and require that a process may request resources only in **increasing order of number**: a process holding resource $i$ may request only a resource $j>i$. (A weaker variant: a process may hold only one resource at a time.)

*Why it works:* in a resource graph, an arrow from a process to a resource goes to a higher-numbered resource, and a resource points to the holding process, so a cycle would need a process to hold a resource with a higher number while waiting for one with a lower number, which is forbidden. Example: number the scanner 1 and the printer 2; every process must request the scanner before the printer, so a process holding the printer can never wait for the scanner, and no cycle of waiting processes can form.
