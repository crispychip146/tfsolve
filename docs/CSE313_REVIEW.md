# CSE 313 question and solution audit

Date: 2026-10-31. Scope: **CSE 313 only**, plus the shared website's requested black styling.

Examined **14 papers**, **414 question files** and **408 existing AI solutions**. Six question files are shared setups for subparts; they do not need separate solutions. Revised **111 solution files** for correctness, completeness, assumptions, diagrams or explanation quality. Other courses' question/solution content was not edited.

This is an **AI content audit**, not human verification or a claim that every remaining answer is proven error-free. All AI solutions retain `status: unverified`. The audit uses the stored questions, their available figures and paper metadata. The original merged source PDF was not supplied in this workspace, so transcription accuracy against that PDF is **not certified**. Original question statements, paper metadata, human attribution and source-rule files are preserved.

The supplied [AI_RULES](../AI_RULES.md), [WORKFLOW](../WORKFLOW.md) and [NOTE_SCHEMA](../NOTE_SCHEMA.md) guide the explanations: establish the situation, explain the method and consequential steps, state assumptions, show intermediate states, interpret results and identify reusable reasoning. These principles were applied within the bank's existing Markdown/front-matter format; no duplicate concept vault was generated.

## Main corrections

- **Wrong answers attached to questions:** 2015-01 Q7(a-c) contained explanations of unrelated topics. Replaced them with weighted three-class scheduling, FAT-based random access and blocking/nonblocking syscall answers.
- **Memory:** corrected process-total versus table-only memory, root-table waste, packed versus page-granular allocation, full 64-bit paging depth, and the distinction between pointer-tree capacity and addressable physical storage.
- **Concurrency:** corrected a deadlock counterexample, supplied a rigorous asymmetric dining-philosopher proof, used the one-fork condition to permit all five diners, repaired unsafe DMA completion/buffer ownership examples, and added atomic ordering assumptions to C-style shared-state examples.
- **Storage:** fixed checksum-journaling ordering, incomplete versus committed transactions, inode-map persistence, unlink/open-reference semantics, overclaims about LFS atomicity, NTFS growth, V7 block geometry and RAID reliability comparisons.
- **Numerical accounting:** corrected SCAN/C-SCAN batch endpoints, coherent TLB invalidation, seek/rotation assumptions, bitmap totals and even-length middle-block indexing. Preserved correct scheduling/page-fault results.
- **Explanation and figures:** added missing thread-placement, Clock, LFS-segment, buddy-allocation, process-tree, process-life-cycle and burst diagrams, and replaced the misleading NTFS figure. Every new/replaced figure has PNG and TikZ source.

## Numerical checkpoints

These are independent calculation/simulation results for content review, **not project/browser test results**. Assumptions and complete working appear in the corresponding solutions.

| Paper / part | Recomputed result |
|:--|:--|
| 2025-09 Q1(a) | Unsafe; P2 then P1 finish, Work=(6,2,5), P3/P4 cannot finish |
| 2025-09 Q2(a) | Mean turnaround 73.75 s |
| 2025-09 Q5(a) | 4 MiB single-level; 6 KiB packed tree, 1936 B unused entries; 84 KiB if one 4 KiB frame per table |
| 2025-09 Q5(b) | Segment size 1.92 MB |
| 2025-09 Q6(a) | Clock: 9 faults, 3 hits |
| 2025-09 Q8(a) | B/C internal waste 240 KiB; free chunks 256+128 KiB, cannot buddy-merge |
| 2025-01 Q1(a) | Mean turnaround 96.25 s under the stated dispatch-count convention |
| 2025-01 Q3(b) | Safe, e.g. P1,P2,P3,P4 |
| 2025-01 Q5(a) | Total resident memory 3136 B / 2432 B; table-only memory 1024 B / 320 B |
| 2025-01 Q5(b) | Segment size 4.5 MB |
| 2025-01 Q7(a) | Inode tree capacity 21483249664 B |
| 2023-10 Q1(a) | SRTN 70 s; priority/RR 91.25 s mean turnaround |
| 2023-10 Q3(a), 2016-01 Q2(b) | Safe sequence P2,P1,P3,P4 |
| 2023-10 Q5(a) | FIFO faults 10/11 and OPT faults 8/6 for 3/4 frames; coherent Clock/TLB total 32001325 ns |
| 2023-10 Q8(a-iv) | With block stripes and 15 ms positioning: segment writes about 810/448 MiB/s, RAID-0/striped mirrored RAID-1 |
| 2023-03 Q3(b) | Mean response 3, mean turnaround 15 time units |
| 2023-03 Q5(a) | 262668 data blocks; simplified 100 MiB read estimate 1.505 s |
| 2023-03 Q6(a) | Immediate next-batch arrival: SSTF 18 tracks, SCAN 65542, C-SCAN 131071; 0.05493/200.024/400.009 ms under linear seeking |
| 2022-04 Q4(b) | SJF 112.5 s, RR 175 s, priority/RR 135 s mean turnaround |
| 2022-04 Q6(a) | Inode capacity 4299210752 B |
| 2021-01 Q1(a) | 17 pages do not fit in 16 with 4096 B pages; 128 pages fit with 512 B pages |
| 2021-01 Q1(c) | Aging final counters 124,73,55,209; evict page 2 |
| 2021-01 Q3(c) | Safe P3,P2,P1; tentative P1 request makes the state unsafe |
| 2021-01 Q8(b) | Original V7 geometry gives tree capacity 1082201088 B |
| 2019-03 Q2(b) | NRU=1, FIFO=2, LRU=0, Second Chance=1 |
| 2019-03 Q4(b) | FCFS 876 ms, SSTF 360 ms, upward LOOK/elevator 348 ms |
| 2019-03 Q5(c) | FCFS mean turnaround/response 8.9/4.9 s; RR(q=2) 10.3/2.1 s |
| 2018-03 Q3(a) | Tree 141012904202240 B, but 32-bit block numbers address at most 8 TiB including metadata |
| 2018-03 Q3(b) | Byte 450000: logical block 219, offset 1488, single-indirect index 209 |
| 2018-03 Q6(c) | FCFS mean turnaround 23.25; throughput 4/36 |
| 2018-03 Q12(b) | Unsafe; only C can be simulated to completion |
| 2017-08 Q3(a) | Tree capacity 4402345713664 B |
| 2017-08 Q7(a) | SJF 53.25; RR(q=8) 77.25 mean turnaround |
| 2016-01 Q1(a) | OPT/FIFO/LRU hits 6/3/5 |
| 2016-01 Q2(c) | RR(q=1,2,4) mean turnaround 11.1/10.3/10.3 s |
| 2016-01 Q5(e) | Tree capacity 17247250432 B; byte 360000 uses double indices 0,85 and byte offset 576 |
| 2015-01 Q2(d) | Inode 555: block 22, byte offset 720 |
| 2015-01 Q3(d) | Tree capacity 282025808400384 B; byte 1000000: block 244, offset 576, single-indirect index 235 |
| 2014-05 / 2015-01 aging | Final counters 240,96,16,64,176,80; evict page 2 |
| 2015-01 Q7(d) | RR mean turnaround 220 s |
| 2014-05 Q7(d) | SJF/SRTN/RR mean turnaround 134/134/224 s |
| 2012-12 Q1(b) | 1082204160 B; central block paths 63,63,55 and 63,63,56 |
| 2012-12 Q6(c) | Original state safe; P5 request must wait |
| 2012-12 Q8(b) | SJF/SRTF/RR mean waiting 72/72/162 s |
| 2012-12 Q8(c) | OPT 9 faults; FIFO 15 faults |

## Source ambiguities retained explicitly

- **2014-05 Q6(e), 2015-01 Q6(e), 2022-04 Q3(a):** A's maximum R5 claim is 3 while only 2 instances exist. No x makes the printed state safe. Hypothetical corrected inputs are labeled rather than substituted into the question.
- **2017-08 Q11(b):** P2 holds two R3 and requests three more, exceeding the system total of four. Detection leaves it unmarked, but the demand is impossible rather than a normal interprocess cycle.
- **2016-01 Q3(b):** the full RR trace lacks a quantum/consistent early timing information. With the post-38 event list treated as exhaustive, P2 runs at 47; with unlisted timeouts allowed, that identity depends on additional information.
- **2019-03 Q2(a):** WSClock starting hand and timestamp-sampling convention are unspecified. Explain the convention and its effect instead of presenting it as universal.
- **2022-04 Section B:** the printed unit header says 1 M=2^10. The original is preserved; calculations use the ordinary binary interpretation explicitly where needed.
- **2023-03 Q2(c) and 2023-10 Q6(a):** printed C snippets have missing/incorrect signatures or arguments. The intended interpretation is stated in the solutions; original snippets are preserved.
- **Scheduling/timing problems:** priority preemption, simultaneous-arrival queue order, global versus per-class quantum accounting, average seek, striping and initial positioning require stated assumptions where the question omits them.
- **2025-09 Q6(c):** writing existing files does not itself require new parent-directory entries; the solution distinguishes updates from creation.

## External clarification sources

Existing textbook/source metadata remains in the solution files. These primary references were consulted to resolve implementation details; they are external clarification, not newly supplied lecturer material:

- [OSTEP: Crash Consistency](https://pages.cs.wisc.edu/~remzi/OSTEP/file-journaling.pdf): journal ordering and recovery.
- [OSTEP: Log-structured File Systems](https://pages.cs.wisc.edu/~remzi/OSTEP/file-lfs.pdf): moving inodes, imap/checkpoints and roll-forward.
- [OSTEP: Paging, Smaller Tables](https://pages.cs.wisc.edu/~remzi/OSTEP/vm-smalltables.pdf): sparse hierarchical mappings.
- [OSTEP: File System Implementation](https://pages.cs.wisc.edu/~remzi/OSTEP/file-implementation.pdf): inode/block bookkeeping.
- [OSTEP: Locality and FFS](https://pages.cs.wisc.edu/~remzi/OSTEP/file-ffs.pdf): placement and large-file exceptions.
- [OSTEP: RAID](https://pages.cs.wisc.edu/~remzi/OSTEP/file-raid.pdf): parity and performance models.
- [OSTEP: Locks](https://pages.cs.wisc.edu/~remzi/OSTEP/threads-locks.pdf): spin/blocking/ticket-lock mechanisms.
- [Microsoft: NTFS attribute records](https://learn.microsoft.com/en-us/windows/win32/devnotes/attribute-record-header) and [MFT](https://learn.microsoft.com/en-us/windows/win32/fileio/master-file-table): runlists, attributes and extension records.
- [Original V7 regeneration documentation, Haley/Ritchie](https://minnie.tuhs.org/PUPS/Setup/v7_regen.html): historical 512-byte buffer geometry.

## Website change and testing handoff

The shared screen palette now uses **#000000** for page, header, hero, card, input, code and status-badge surfaces. Links/controls are blue and headings/details are purple. Removed light-mode toggling so device or saved light preferences cannot override the requested black canvas. Technical figures are inverted in screen CSS and retain their original colors when printed; print pages remain white to avoid black PDF/paper backgrounds.

**No project tests, browser tests, website build or PDF build were run**, as requested. Numerical recomputation and source/diff inspection were performed for the content audit. Browser/PDF rendering and the normal project checks are left to the user. TikZ sources and PNGs are supplied together; new PNGs were drawn from the same diagram data rather than compiled with LaTeX in this environment.

## Per-question coverage and changes

"Examined; no correction identified" means the existing answer was retained after this AI pass. It does not change the human-verification badge. Every actual solution edit also has a dated `changes:` entry.

| Paper | Question files | Solutions | Revised solutions |
|:--|:-:|:-:|:-:|
| 2012-12 | 28 | 26 | 6 |
| 2014-05 | 33 | 33 | 5 |
| 2015-01 | 34 | 34 | 9 |
| 2016-01 | 34 | 34 | 6 |
| 2016-07 | 28 | 27 | 7 |
| 2017-08 | 37 | 37 | 5 |
| 2018-03 | 36 | 36 | 7 |
| 2019-03 | 30 | 30 | 5 |
| 2021-01 | 21 | 21 | 6 |
| 2022-04 | 27 | 26 | 7 |
| 2023-03 | 25 | 25 | 13 |
| 2023-10 | 27 | 25 | 10 |
| 2025-01 | 28 | 28 | 11 |
| 2025-09 | 26 | 26 | 14 |

### 2012-12

| Part | Solution | Audit outcome |
|:--|:--|:--|
| [q1a](../bank/CSE/CSE313/2012-12/q1a.md) | [AI solution](../bank/CSE/CSE313/2012-12/solutions/q1a/ai.md) | Examined; no correction identified. |
| [q1b](../bank/CSE/CSE313/2012-12/q1b.md) | [AI solution](../bank/CSE/CSE313/2012-12/solutions/q1b/ai.md) | Revised: Identified both central blocks of an even-length file. |
| [q1c](../bank/CSE/CSE313/2012-12/q1c.md) | [AI solution](../bank/CSE/CSE313/2012-12/solutions/q1c/ai.md) | Examined; no correction identified. |
| [q2a](../bank/CSE/CSE313/2012-12/q2a.md) | [AI solution](../bank/CSE/CSE313/2012-12/solutions/q2a/ai.md) | Examined; no correction identified. |
| [q2b](../bank/CSE/CSE313/2012-12/q2b.md) | [AI solution](../bank/CSE/CSE313/2012-12/solutions/q2b/ai.md) | Examined; no correction identified. |
| [q2c](../bank/CSE/CSE313/2012-12/q2c.md) | [AI solution](../bank/CSE/CSE313/2012-12/solutions/q2c/ai.md) | Examined; no correction identified. |
| [q3](../bank/CSE/CSE313/2012-12/q3.md) | Shared setup | Examined with its child parts; no standalone answer required. |
| [q3a](../bank/CSE/CSE313/2012-12/q3a.md) | [AI solution](../bank/CSE/CSE313/2012-12/solutions/q3a/ai.md) | Revised: Used one consistent cache lock and explained why an ordinary interrupt-context lock cannot replace interrupt masking. |
| [q3b](../bank/CSE/CSE313/2012-12/q3b.md) | [AI solution](../bank/CSE/CSE313/2012-12/solutions/q3b/ai.md) | Examined; no correction identified. |
| [q3c](../bank/CSE/CSE313/2012-12/q3c.md) | [AI solution](../bank/CSE/CSE313/2012-12/solutions/q3c/ai.md) | Examined; no correction identified. |
| [q3d](../bank/CSE/CSE313/2012-12/q3d.md) | [AI solution](../bank/CSE/CSE313/2012-12/solutions/q3d/ai.md) | Revised: Corrected the 32 KiB throughput example and stated random-I/O assumptions. |
| [q4a](../bank/CSE/CSE313/2012-12/q4a.md) | [AI solution](../bank/CSE/CSE313/2012-12/solutions/q4a/ai.md) | Examined; no correction identified. |
| [q4b](../bank/CSE/CSE313/2012-12/q4b.md) | [AI solution](../bank/CSE/CSE313/2012-12/solutions/q4b/ai.md) | Examined; no correction identified. |
| [q4c](../bank/CSE/CSE313/2012-12/q4c.md) | [AI solution](../bank/CSE/CSE313/2012-12/solutions/q4c/ai.md) | Examined; no correction identified. |
| [q5](../bank/CSE/CSE313/2012-12/q5.md) | Shared setup | Examined with its child parts; no standalone answer required. |
| [q5a](../bank/CSE/CSE313/2012-12/q5a.md) | [AI solution](../bank/CSE/CSE313/2012-12/solutions/q5a/ai.md) | Examined; no correction identified. |
| [q5b](../bank/CSE/CSE313/2012-12/q5b.md) | [AI solution](../bank/CSE/CSE313/2012-12/solutions/q5b/ai.md) | Revised: Qualified starvation freedom by semaphore and scheduler fairness. |
| [q6a](../bank/CSE/CSE313/2012-12/q6a.md) | [AI solution](../bank/CSE/CSE313/2012-12/solutions/q6a/ai.md) | Examined; no correction identified. |
| [q6b](../bank/CSE/CSE313/2012-12/q6b.md) | [AI solution](../bank/CSE/CSE313/2012-12/solutions/q6b/ai.md) | Examined; no correction identified. |
| [q6c](../bank/CSE/CSE313/2012-12/q6c.md) | [AI solution](../bank/CSE/CSE313/2012-12/solutions/q6c/ai.md) | Examined; no correction identified. |
| [q6d](../bank/CSE/CSE313/2012-12/q6d.md) | [AI solution](../bank/CSE/CSE313/2012-12/solutions/q6d/ai.md) | Examined; no correction identified. |
| [q7a](../bank/CSE/CSE313/2012-12/q7a.md) | [AI solution](../bank/CSE/CSE313/2012-12/solutions/q7a/ai.md) | Revised: Separated classic trap-and-emulate privilege from hardware-assisted guest privilege. |
| [q7b](../bank/CSE/CSE313/2012-12/q7b.md) | [AI solution](../bank/CSE/CSE313/2012-12/solutions/q7b/ai.md) | Revised: Corrected all-operations-require-syscalls and fixed-thread-table claims. |
| [q7c](../bank/CSE/CSE313/2012-12/q7c.md) | [AI solution](../bank/CSE/CSE313/2012-12/solutions/q7c/ai.md) | Examined; no correction identified. |
| [q7d](../bank/CSE/CSE313/2012-12/q7d.md) | [AI solution](../bank/CSE/CSE313/2012-12/solutions/q7d/ai.md) | Examined; no correction identified. |
| [q8a](../bank/CSE/CSE313/2012-12/q8a.md) | [AI solution](../bank/CSE/CSE313/2012-12/solutions/q8a/ai.md) | Examined; no correction identified. |
| [q8b](../bank/CSE/CSE313/2012-12/q8b.md) | [AI solution](../bank/CSE/CSE313/2012-12/solutions/q8b/ai.md) | Examined; no correction identified. |
| [q8c](../bank/CSE/CSE313/2012-12/q8c.md) | [AI solution](../bank/CSE/CSE313/2012-12/solutions/q8c/ai.md) | Examined; no correction identified. |

### 2014-05

| Part | Solution | Audit outcome |
|:--|:--|:--|
| [q1a](../bank/CSE/CSE313/2014-05/q1a.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q1a/ai.md) | Examined; no correction identified. |
| [q1b](../bank/CSE/CSE313/2014-05/q1b.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q1b/ai.md) | Examined; no correction identified. |
| [q1c](../bank/CSE/CSE313/2014-05/q1c.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q1c/ai.md) | Revised: Qualified the preemptible-kernel race rather than contradicting historical nonpreemptive UNIX. |
| [q1d](../bank/CSE/CSE313/2014-05/q1d.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q1d/ai.md) | Examined; no correction identified. |
| [q2a](../bank/CSE/CSE313/2014-05/q2a.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q2a/ai.md) | Examined; no correction identified. |
| [q2b](../bank/CSE/CSE313/2014-05/q2b.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q2b/ai.md) | Examined; no correction identified. |
| [q2c](../bank/CSE/CSE313/2014-05/q2c.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q2c/ai.md) | Examined; no correction identified. |
| [q2d](../bank/CSE/CSE313/2014-05/q2d.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q2d/ai.md) | Examined; no correction identified. |
| [q3a](../bank/CSE/CSE313/2014-05/q3a.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q3a/ai.md) | Examined; no correction identified. |
| [q3b](../bank/CSE/CSE313/2014-05/q3b.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q3b/ai.md) | Examined; no correction identified. |
| [q3c](../bank/CSE/CSE313/2014-05/q3c.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q3c/ai.md) | Examined; no correction identified. |
| [q3d](../bank/CSE/CSE313/2014-05/q3d.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q3d/ai.md) | Examined; no correction identified. |
| [q4a](../bank/CSE/CSE313/2014-05/q4a.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q4a/ai.md) | Examined; no correction identified. |
| [q4b](../bank/CSE/CSE313/2014-05/q4b.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q4b/ai.md) | Examined; no correction identified. |
| [q4c](../bank/CSE/CSE313/2014-05/q4c.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q4c/ai.md) | Examined; no correction identified. |
| [q4d](../bank/CSE/CSE313/2014-05/q4d.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q4d/ai.md) | Examined; no correction identified. |
| [q5a](../bank/CSE/CSE313/2014-05/q5a.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q5a/ai.md) | Revised: Removed the claim that space-saving table layouts inherently speed translations. |
| [q5b](../bank/CSE/CSE313/2014-05/q5b.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q5b/ai.md) | Examined; no correction identified. |
| [q5c](../bank/CSE/CSE313/2014-05/q5c.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q5c/ai.md) | Examined; no correction identified. |
| [q6a](../bank/CSE/CSE313/2014-05/q6a.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q6a/ai.md) | Examined; no correction identified. |
| [q6b](../bank/CSE/CSE313/2014-05/q6b.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q6b/ai.md) | Revised: Removed a printer example that described completion rather than resource preemption. |
| [q6c](../bank/CSE/CSE313/2014-05/q6c.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q6c/ai.md) | Examined; no correction identified. |
| [q6d](../bank/CSE/CSE313/2014-05/q6d.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q6d/ai.md) | Examined; no correction identified. |
| [q6e](../bank/CSE/CSE313/2014-05/q6e.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q6e/ai.md) | Revised: Corrected resource totals while retaining the no-safe-x conclusion. |
| [q7a](../bank/CSE/CSE313/2014-05/q7a.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q7a/ai.md) | Examined; no correction identified. |
| [q7b](../bank/CSE/CSE313/2014-05/q7b.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q7b/ai.md) | Revised: Distinguished probabilistic lottery fairness from deterministic starvation bounds. |
| [q7c](../bank/CSE/CSE313/2014-05/q7c.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q7c/ai.md) | Examined; no correction identified. |
| [q7d](../bank/CSE/CSE313/2014-05/q7d.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q7d/ai.md) | Examined; no correction identified. |
| [q8a](../bank/CSE/CSE313/2014-05/q8a.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q8a/ai.md) | Examined; no correction identified. |
| [q8b](../bank/CSE/CSE313/2014-05/q8b.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q8b/ai.md) | Examined; no correction identified. |
| [q8c](../bank/CSE/CSE313/2014-05/q8c.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q8c/ai.md) | Examined; no correction identified. |
| [q8d](../bank/CSE/CSE313/2014-05/q8d.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q8d/ai.md) | Examined; no correction identified. |
| [q8e](../bank/CSE/CSE313/2014-05/q8e.md) | [AI solution](../bank/CSE/CSE313/2014-05/solutions/q8e/ai.md) | Examined; no correction identified. |

### 2015-01

| Part | Solution | Audit outcome |
|:--|:--|:--|
| [q1a](../bank/CSE/CSE313/2015-01/q1a.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q1a/ai.md) | Examined; no correction identified. |
| [q1b](../bank/CSE/CSE313/2015-01/q1b.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q1b/ai.md) | Examined; no correction identified. |
| [q1c](../bank/CSE/CSE313/2015-01/q1c.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q1c/ai.md) | Examined; no correction identified. |
| [q1d](../bank/CSE/CSE313/2015-01/q1d.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q1d/ai.md) | Examined; no correction identified. |
| [q2a](../bank/CSE/CSE313/2015-01/q2a.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q2a/ai.md) | Examined; no correction identified. |
| [q2b](../bank/CSE/CSE313/2015-01/q2b.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q2b/ai.md) | Examined; no correction identified. |
| [q2c](../bank/CSE/CSE313/2015-01/q2c.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q2c/ai.md) | Revised: Made the promised inode lock explicit before returning or reading a reassigned inode. |
| [q2d](../bank/CSE/CSE313/2015-01/q2d.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q2d/ai.md) | Examined; no correction identified. |
| [q3a](../bank/CSE/CSE313/2015-01/q3a.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q3a/ai.md) | Examined; no correction identified. |
| [q3b](../bank/CSE/CSE313/2015-01/q3b.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q3b/ai.md) | Examined; no correction identified. |
| [q3c](../bank/CSE/CSE313/2015-01/q3c.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q3c/ai.md) | Examined; no correction identified. |
| [q3d](../bank/CSE/CSE313/2015-01/q3d.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q3d/ai.md) | Examined; no correction identified. |
| [q4a](../bank/CSE/CSE313/2015-01/q4a.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q4a/ai.md) | Revised: Clarified kernel sleep priority versus processor interrupt level and user sleep API. |
| [q4b](../bank/CSE/CSE313/2015-01/q4b.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q4b/ai.md) | Revised: Clarified that static system context does not mean all its tables have fixed lifetime sizes. |
| [q4c](../bank/CSE/CSE313/2015-01/q4c.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q4c/ai.md) | Examined; no correction identified. |
| [q4d](../bank/CSE/CSE313/2015-01/q4d.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q4d/ai.md) | Examined; no correction identified. |
| [q5a](../bank/CSE/CSE313/2015-01/q5a.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q5a/ai.md) | Examined; no correction identified. |
| [q5b](../bank/CSE/CSE313/2015-01/q5b.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q5b/ai.md) | Examined; no correction identified. |
| [q5c](../bank/CSE/CSE313/2015-01/q5c.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q5c/ai.md) | Examined; no correction identified. |
| [q5d](../bank/CSE/CSE313/2015-01/q5d.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q5d/ai.md) | Revised: Corrected worst-case multilevel-table coverage and explained sparse-region costs. |
| [q6a](../bank/CSE/CSE313/2015-01/q6a.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q6a/ai.md) | Examined; no correction identified. |
| [q6b](../bank/CSE/CSE313/2015-01/q6b.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q6b/ai.md) | Examined; no correction identified. |
| [q6c](../bank/CSE/CSE313/2015-01/q6c.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q6c/ai.md) | Examined; no correction identified. |
| [q6d](../bank/CSE/CSE313/2015-01/q6d.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q6d/ai.md) | Examined; no correction identified. |
| [q6e](../bank/CSE/CSE313/2015-01/q6e.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q6e/ai.md) | Revised: Corrected resource totals while retaining the no-safe-x conclusion. |
| [q7a](../bank/CSE/CSE313/2015-01/q7a.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q7a/ai.md) | Revised: Replaced an unrelated real-time answer with the requested weighted three-class scheduler. |
| [q7b](../bank/CSE/CSE313/2015-01/q7b.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q7b/ai.md) | Revised: Replaced unrelated scheduling notes with the FAT-in-memory modification and its random-access cost. |
| [q7c](../bank/CSE/CSE313/2015-01/q7c.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q7c/ai.md) | Revised: Replaced unrelated CPU/I/O-bound notes with blocking versus nonblocking syscall semantics. |
| [q7d](../bank/CSE/CSE313/2015-01/q7d.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q7d/ai.md) | Examined; no correction identified. |
| [q7e](../bank/CSE/CSE313/2015-01/q7e.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q7e/ai.md) | Examined; no correction identified. |
| [q8a](../bank/CSE/CSE313/2015-01/q8a.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q8a/ai.md) | Examined; no correction identified. |
| [q8b](../bank/CSE/CSE313/2015-01/q8b.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q8b/ai.md) | Examined; no correction identified. |
| [q8c](../bank/CSE/CSE313/2015-01/q8c.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q8c/ai.md) | Examined; no correction identified. |
| [q8d](../bank/CSE/CSE313/2015-01/q8d.md) | [AI solution](../bank/CSE/CSE313/2015-01/solutions/q8d/ai.md) | Revised: Used the one-fork requirement to achieve the full five-philosopher parallelism directly. |

### 2016-01

| Part | Solution | Audit outcome |
|:--|:--|:--|
| [q1a](../bank/CSE/CSE313/2016-01/q1a.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q1a/ai.md) | Examined; no correction identified. |
| [q1b](../bank/CSE/CSE313/2016-01/q1b.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q1b/ai.md) | Examined; no correction identified. |
| [q1c](../bank/CSE/CSE313/2016-01/q1c.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q1c/ai.md) | Revised: Corrected worst-case multilevel-table coverage and explained sparse-region costs. |
| [q2a](../bank/CSE/CSE313/2016-01/q2a.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q2a/ai.md) | Examined; no correction identified. |
| [q2b](../bank/CSE/CSE313/2016-01/q2b.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q2b/ai.md) | Examined; no correction identified. |
| [q2c](../bank/CSE/CSE313/2016-01/q2c.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q2c/ai.md) | Examined; no correction identified. |
| [q3a](../bank/CSE/CSE313/2016-01/q3a.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q3a/ai.md) | Examined; no correction identified. |
| [q3b](../bank/CSE/CSE313/2016-01/q3b.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q3b/ai.md) | Revised: Resolved the inconsistent time-47 conclusion under the exhaustive-events assumption. |
| [q3c](../bank/CSE/CSE313/2016-01/q3c.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q3c/ai.md) | Revised: Separated standard Second Chance sweep updates from periodic sampled-reference variants. |
| [q4a](../bank/CSE/CSE313/2016-01/q4a.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q4a/ai.md) | Examined; no correction identified. |
| [q4b](../bank/CSE/CSE313/2016-01/q4b.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q4b/ai.md) | Examined; no correction identified. |
| [q4c](../bank/CSE/CSE313/2016-01/q4c.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q4c/ai.md) | Examined; no correction identified. |
| [q5a](../bank/CSE/CSE313/2016-01/q5a.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q5a/ai.md) | Examined; no correction identified. |
| [q5b](../bank/CSE/CSE313/2016-01/q5b.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q5b/ai.md) | Examined; no correction identified. |
| [q5c](../bank/CSE/CSE313/2016-01/q5c.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q5c/ai.md) | Revised: Made the promised inode lock explicit before returning or reading a reassigned inode. |
| [q5d](../bank/CSE/CSE313/2016-01/q5d.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q5d/ai.md) | Examined; no correction identified. |
| [q5e](../bank/CSE/CSE313/2016-01/q5e.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q5e/ai.md) | Examined; no correction identified. |
| [q5f](../bank/CSE/CSE313/2016-01/q5f.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q5f/ai.md) | Examined; no correction identified. |
| [q6a](../bank/CSE/CSE313/2016-01/q6a.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q6a/ai.md) | Revised: Distinguished UNIX-like interfaces, certification and licensing. |
| [q6b](../bank/CSE/CSE313/2016-01/q6b.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q6b/ai.md) | Examined; no correction identified. |
| [q6c](../bank/CSE/CSE313/2016-01/q6c.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q6c/ai.md) | Examined; no correction identified. |
| [q6d](../bank/CSE/CSE313/2016-01/q6d.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q6d/ai.md) | Examined; no correction identified. |
| [q6e](../bank/CSE/CSE313/2016-01/q6e.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q6e/ai.md) | Examined; no correction identified. |
| [q7a](../bank/CSE/CSE313/2016-01/q7a.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q7a/ai.md) | Examined; no correction identified. |
| [q7b](../bank/CSE/CSE313/2016-01/q7b.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q7b/ai.md) | Revised: Kept linked-list allocation consistent with its pointer overhead in the preceding part. |
| [q7c](../bank/CSE/CSE313/2016-01/q7c.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q7c/ai.md) | Examined; no correction identified. |
| [q7d](../bank/CSE/CSE313/2016-01/q7d.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q7d/ai.md) | Examined; no correction identified. |
| [q7e](../bank/CSE/CSE313/2016-01/q7e.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q7e/ai.md) | Examined; no correction identified. |
| [q7f](../bank/CSE/CSE313/2016-01/q7f.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q7f/ai.md) | Examined; no correction identified. |
| [q8a](../bank/CSE/CSE313/2016-01/q8a.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q8a/ai.md) | Examined; no correction identified. |
| [q8b](../bank/CSE/CSE313/2016-01/q8b.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q8b/ai.md) | Examined; no correction identified. |
| [q8c](../bank/CSE/CSE313/2016-01/q8c.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q8c/ai.md) | Examined; no correction identified. |
| [q8d](../bank/CSE/CSE313/2016-01/q8d.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q8d/ai.md) | Examined; no correction identified. |
| [q8e](../bank/CSE/CSE313/2016-01/q8e.md) | [AI solution](../bank/CSE/CSE313/2016-01/solutions/q8e/ai.md) | Examined; no correction identified. |

### 2016-07

| Part | Solution | Audit outcome |
|:--|:--|:--|
| [q1a](../bank/CSE/CSE313/2016-07/q1a.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q1a/ai.md) | Examined; no correction identified. |
| [q1b](../bank/CSE/CSE313/2016-07/q1b.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q1b/ai.md) | Examined; no correction identified. |
| [q1c](../bank/CSE/CSE313/2016-07/q1c.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q1c/ai.md) | Examined; no correction identified. |
| [q2a](../bank/CSE/CSE313/2016-07/q2a.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q2a/ai.md) | Examined; no correction identified. |
| [q2b](../bank/CSE/CSE313/2016-07/q2b.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q2b/ai.md) | Examined; no correction identified. |
| [q2c](../bank/CSE/CSE313/2016-07/q2c.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q2c/ai.md) | Examined; no correction identified. |
| [q3a](../bank/CSE/CSE313/2016-07/q3a.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q3a/ai.md) | Examined; no correction identified. |
| [q3b](../bank/CSE/CSE313/2016-07/q3b.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q3b/ai.md) | Examined; no correction identified. |
| [q3c](../bank/CSE/CSE313/2016-07/q3c.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q3c/ai.md) | Examined; no correction identified. |
| [q4a](../bank/CSE/CSE313/2016-07/q4a.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q4a/ai.md) | Examined; no correction identified. |
| [q4b](../bank/CSE/CSE313/2016-07/q4b.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q4b/ai.md) | Examined; no correction identified. |
| [q4c](../bank/CSE/CSE313/2016-07/q4c.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q4c/ai.md) | Revised: Included explicit flush/coherence write-back and clarified buffered write-through. |
| [q4d](../bank/CSE/CSE313/2016-07/q4d.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q4d/ai.md) | Examined; no correction identified. |
| [q5a](../bank/CSE/CSE313/2016-07/q5a.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q5a/ai.md) | Revised: Corrected NTFS attribute-list/runlist growth and replaced the misleading extent-tree diagram. |
| [q5b](../bank/CSE/CSE313/2016-07/q5b.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q5b/ai.md) | Examined; no correction identified. |
| [q5c](../bank/CSE/CSE313/2016-07/q5c.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q5c/ai.md) | Examined; no correction identified. |
| [q6a](../bank/CSE/CSE313/2016-07/q6a.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q6a/ai.md) | Revised: Replaced universal allocator claims with the actual locality mechanisms and their limits. |
| [q6b](../bank/CSE/CSE313/2016-07/q6b.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q6b/ai.md) | Revised: Removed the requirement that the entire FAT must fit in RAM. |
| [q6c](../bank/CSE/CSE313/2016-07/q6c.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q6c/ai.md) | Examined; no correction identified. |
| [q7](../bank/CSE/CSE313/2016-07/q7.md) | Shared setup | Examined with its child parts; no standalone answer required. |
| [q7a](../bank/CSE/CSE313/2016-07/q7a.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q7a/ai.md) | Revised: Removed an invented output fragment and corrected printf/join assumptions. |
| [q7b](../bank/CSE/CSE313/2016-07/q7b.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q7b/ai.md) | Examined; no correction identified. |
| [q7c](../bank/CSE/CSE313/2016-07/q7c.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q7c/ai.md) | Examined; no correction identified. |
| [q7d](../bank/CSE/CSE313/2016-07/q7d.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q7d/ai.md) | Revised: Explained retained semaphore notifications without claiming sequential P and V commute. |
| [q8a](../bank/CSE/CSE313/2016-07/q8a.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q8a/ai.md) | Revised: Used explicit acquire/release atomics and scoped interrupt masking to IRQ-shared critical sections. |
| [q8b](../bank/CSE/CSE313/2016-07/q8b.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q8b/ai.md) | Examined; no correction identified. |
| [q8c](../bank/CSE/CSE313/2016-07/q8c.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q8c/ai.md) | Examined; no correction identified. |
| [q8d](../bank/CSE/CSE313/2016-07/q8d.md) | [AI solution](../bank/CSE/CSE313/2016-07/solutions/q8d/ai.md) | Examined; no correction identified. |

### 2017-08

| Part | Solution | Audit outcome |
|:--|:--|:--|
| [q10a](../bank/CSE/CSE313/2017-08/q10a.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q10a/ai.md) | Examined; no correction identified. |
| [q10b](../bank/CSE/CSE313/2017-08/q10b.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q10b/ai.md) | Examined; no correction identified. |
| [q10c](../bank/CSE/CSE313/2017-08/q10c.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q10c/ai.md) | Examined; no correction identified. |
| [q11a](../bank/CSE/CSE313/2017-08/q11a.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q11a/ai.md) | Examined; no correction identified. |
| [q11b](../bank/CSE/CSE313/2017-08/q11b.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q11b/ai.md) | Revised: Flagged an impossible request while retaining the detection algorithm result. |
| [q12a](../bank/CSE/CSE313/2017-08/q12a.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q12a/ai.md) | Examined; no correction identified. |
| [q12b](../bank/CSE/CSE313/2017-08/q12b.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q12b/ai.md) | Examined; no correction identified. |
| [q13a](../bank/CSE/CSE313/2017-08/q13a.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q13a/ai.md) | Examined; no correction identified. |
| [q13b](../bank/CSE/CSE313/2017-08/q13b.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q13b/ai.md) | Examined; no correction identified. |
| [q13c](../bank/CSE/CSE313/2017-08/q13c.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q13c/ai.md) | Examined; no correction identified. |
| [q1a](../bank/CSE/CSE313/2017-08/q1a.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q1a/ai.md) | Revised: Distinguished fault-isolation risks from module support in monolithic kernels. |
| [q1b](../bank/CSE/CSE313/2017-08/q1b.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q1b/ai.md) | Examined; no correction identified. |
| [q1c](../bank/CSE/CSE313/2017-08/q1c.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q1c/ai.md) | Examined; no correction identified. |
| [q1d](../bank/CSE/CSE313/2017-08/q1d.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q1d/ai.md) | Examined; no correction identified. |
| [q2a](../bank/CSE/CSE313/2017-08/q2a.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q2a/ai.md) | Examined; no correction identified. |
| [q2b](../bank/CSE/CSE313/2017-08/q2b.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q2b/ai.md) | Examined; no correction identified. |
| [q2c](../bank/CSE/CSE313/2017-08/q2c.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q2c/ai.md) | Examined; no correction identified. |
| [q2d](../bank/CSE/CSE313/2017-08/q2d.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q2d/ai.md) | Examined; no correction identified. |
| [q3a](../bank/CSE/CSE313/2017-08/q3a.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q3a/ai.md) | Examined; no correction identified. |
| [q3b](../bank/CSE/CSE313/2017-08/q3b.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q3b/ai.md) | Examined; no correction identified. |
| [q3c](../bank/CSE/CSE313/2017-08/q3c.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q3c/ai.md) | Examined; no correction identified. |
| [q3d](../bank/CSE/CSE313/2017-08/q3d.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q3d/ai.md) | Examined; no correction identified. |
| [q4a](../bank/CSE/CSE313/2017-08/q4a.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q4a/ai.md) | Examined; no correction identified. |
| [q4b](../bank/CSE/CSE313/2017-08/q4b.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q4b/ai.md) | Examined; no correction identified. |
| [q4c](../bank/CSE/CSE313/2017-08/q4c.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q4c/ai.md) | Examined; no correction identified. |
| [q4d](../bank/CSE/CSE313/2017-08/q4d.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q4d/ai.md) | Examined; no correction identified. |
| [q5a](../bank/CSE/CSE313/2017-08/q5a.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q5a/ai.md) | Examined; no correction identified. |
| [q5b](../bank/CSE/CSE313/2017-08/q5b.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q5b/ai.md) | Revised: Stated integer range/overflow assumptions and the closed-form alternative to exhaustive summation. |
| [q5c](../bank/CSE/CSE313/2017-08/q5c.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q5c/ai.md) | Examined; no correction identified. |
| [q6a](../bank/CSE/CSE313/2017-08/q6a.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q6a/ai.md) | Examined; no correction identified. |
| [q6b](../bank/CSE/CSE313/2017-08/q6b.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q6b/ai.md) | Examined; no correction identified. |
| [q6c](../bank/CSE/CSE313/2017-08/q6c.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q6c/ai.md) | Examined; no correction identified. |
| [q7a](../bank/CSE/CSE313/2017-08/q7a.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q7a/ai.md) | Examined; no correction identified. |
| [q7b](../bank/CSE/CSE313/2017-08/q7b.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q7b/ai.md) | Revised: Clarified SRTN optimality without requiring knowledge of future arrivals. |
| [q8a](../bank/CSE/CSE313/2017-08/q8a.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q8a/ai.md) | Examined; no correction identified. |
| [q9a](../bank/CSE/CSE313/2017-08/q9a.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q9a/ai.md) | Examined; no correction identified. |
| [q9b](../bank/CSE/CSE313/2017-08/q9b.md) | [AI solution](../bank/CSE/CSE313/2017-08/solutions/q9b/ai.md) | Revised: Removed a printer example that described completion rather than resource preemption. |

### 2018-03

| Part | Solution | Audit outcome |
|:--|:--|:--|
| [q10a](../bank/CSE/CSE313/2018-03/q10a.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q10a/ai.md) | Revised: Corrected 64-bit paging depth and finite-resource claims. Placed the short index at the root in the full-width illustrative page-table split. |
| [q10b](../bank/CSE/CSE313/2018-03/q10b.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q10b/ai.md) | Revised: Qualified the textbook ostrich-policy comparison instead of asserting all OS subsystems ignore deadlock. |
| [q11a](../bank/CSE/CSE313/2018-03/q11a.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q11a/ai.md) | Examined; no correction identified. |
| [q12a](../bank/CSE/CSE313/2018-03/q12a.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q12a/ai.md) | Examined; no correction identified. |
| [q12b](../bank/CSE/CSE313/2018-03/q12b.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q12b/ai.md) | Examined; no correction identified. |
| [q13a](../bank/CSE/CSE313/2018-03/q13a.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q13a/ai.md) | Examined; no correction identified. |
| [q13b](../bank/CSE/CSE313/2018-03/q13b.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q13b/ai.md) | Examined; no correction identified. |
| [q1a](../bank/CSE/CSE313/2018-03/q1a.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q1a/ai.md) | Examined; no correction identified. |
| [q1b](../bank/CSE/CSE313/2018-03/q1b.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q1b/ai.md) | Examined; no correction identified. |
| [q1c](../bank/CSE/CSE313/2018-03/q1c.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q1c/ai.md) | Examined; no correction identified. |
| [q1d](../bank/CSE/CSE313/2018-03/q1d.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q1d/ai.md) | Examined; no correction identified. |
| [q2a](../bank/CSE/CSE313/2018-03/q2a.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q2a/ai.md) | Examined; no correction identified. |
| [q2b](../bank/CSE/CSE313/2018-03/q2b.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q2b/ai.md) | Examined; no correction identified. |
| [q2c](../bank/CSE/CSE313/2018-03/q2c.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q2c/ai.md) | Examined; no correction identified. |
| [q2d](../bank/CSE/CSE313/2018-03/q2d.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q2d/ai.md) | Examined; no correction identified. |
| [q3a](../bank/CSE/CSE313/2018-03/q3a.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q3a/ai.md) | Revised: Distinguished pointer-tree capacity from the 32-bit physical block-address limit. Distinguished logical sparse-file limits from fully allocated data capacity. |
| [q3b](../bank/CSE/CSE313/2018-03/q3b.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q3b/ai.md) | Examined; no correction identified. |
| [q3c](../bank/CSE/CSE313/2018-03/q3c.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q3c/ai.md) | Examined; no correction identified. |
| [q3d](../bank/CSE/CSE313/2018-03/q3d.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q3d/ai.md) | Examined; no correction identified. |
| [q4a](../bank/CSE/CSE313/2018-03/q4a.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q4a/ai.md) | Examined; no correction identified. |
| [q4b](../bank/CSE/CSE313/2018-03/q4b.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q4b/ai.md) | Examined; no correction identified. |
| [q4c](../bank/CSE/CSE313/2018-03/q4c.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q4c/ai.md) | Examined; no correction identified. |
| [q4d](../bank/CSE/CSE313/2018-03/q4d.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q4d/ai.md) | Examined; no correction identified. |
| [q5a](../bank/CSE/CSE313/2018-03/q5a.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q5a/ai.md) | Revised: Stated independence assumptions behind the CPU-utilization approximation. |
| [q5b](../bank/CSE/CSE313/2018-03/q5b.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q5b/ai.md) | Revised: Qualified classic thread-model assumptions and user-space synchronization costs. |
| [q6a](../bank/CSE/CSE313/2018-03/q6a.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q6a/ai.md) | Examined; no correction identified. |
| [q6b](../bank/CSE/CSE313/2018-03/q6b.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q6b/ai.md) | Examined; no correction identified. |
| [q6c](../bank/CSE/CSE313/2018-03/q6c.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q6c/ai.md) | Examined; no correction identified. |
| [q7a](../bank/CSE/CSE313/2018-03/q7a.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q7a/ai.md) | Revised: Distinguished the course deadlock-avoidance retry variant from standard database two-phase locking. |
| [q7b](../bank/CSE/CSE313/2018-03/q7b.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q7b/ai.md) | Examined; no correction identified. |
| [q7c](../bank/CSE/CSE313/2018-03/q7c.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q7c/ai.md) | Examined; no correction identified. |
| [q8a](../bank/CSE/CSE313/2018-03/q8a.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q8a/ai.md) | Examined; no correction identified. |
| [q8b](../bank/CSE/CSE313/2018-03/q8b.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q8b/ai.md) | Examined; no correction identified. |
| [q8c](../bank/CSE/CSE313/2018-03/q8c.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q8c/ai.md) | Examined; no correction identified. |
| [q9a](../bank/CSE/CSE313/2018-03/q9a.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q9a/ai.md) | Examined; no correction identified. |
| [q9b](../bank/CSE/CSE313/2018-03/q9b.md) | [AI solution](../bank/CSE/CSE313/2018-03/solutions/q9b/ai.md) | Revised: Required copying memory when relocating and checked virtual offsets against length. |

### 2019-03

| Part | Solution | Audit outcome |
|:--|:--|:--|
| [q1a](../bank/CSE/CSE313/2019-03/q1a.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q1a/ai.md) | Examined; no correction identified. |
| [q1b](../bank/CSE/CSE313/2019-03/q1b.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q1b/ai.md) | Examined; no correction identified. |
| [q1c](../bank/CSE/CSE313/2019-03/q1c.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q1c/ai.md) | Examined; no correction identified. |
| [q1d](../bank/CSE/CSE313/2019-03/q1d.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q1d/ai.md) | Examined; no correction identified. |
| [q2a](../bank/CSE/CSE313/2019-03/q2a.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q2a/ai.md) | Revised: Explained timestamp-sampling variants of WSClock instead of presenting one as universal. |
| [q2b](../bank/CSE/CSE313/2019-03/q2b.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q2b/ai.md) | Examined; no correction identified. |
| [q2c](../bank/CSE/CSE313/2019-03/q2c.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q2c/ai.md) | Examined; no correction identified. |
| [q2d](../bank/CSE/CSE313/2019-03/q2d.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q2d/ai.md) | Examined; no correction identified. |
| [q3a](../bank/CSE/CSE313/2019-03/q3a.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q3a/ai.md) | Examined; no correction identified. |
| [q3b](../bank/CSE/CSE313/2019-03/q3b.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q3b/ai.md) | Revised: Distinguished outstanding requests used in detection from maximum claims used in avoidance. |
| [q3c](../bank/CSE/CSE313/2019-03/q3c.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q3c/ai.md) | Examined; no correction identified. |
| [q3d](../bank/CSE/CSE313/2019-03/q3d.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q3d/ai.md) | Examined; no correction identified. |
| [q4a](../bank/CSE/CSE313/2019-03/q4a.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q4a/ai.md) | Revised: Initialized interrupt-transfer state correctly and used a completion semaphore to avoid lost wakeups. |
| [q4b](../bank/CSE/CSE313/2019-03/q4b.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q4b/ai.md) | Examined; no correction identified. |
| [q4c](../bank/CSE/CSE313/2019-03/q4c.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q4c/ai.md) | Examined; no correction identified. |
| [q4d](../bank/CSE/CSE313/2019-03/q4d.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q4d/ai.md) | Examined; no correction identified. |
| [q5a](../bank/CSE/CSE313/2019-03/q5a.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q5a/ai.md) | Examined; no correction identified. |
| [q5b](../bank/CSE/CSE313/2019-03/q5b.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q5b/ai.md) | Examined; no correction identified. |
| [q5c](../bank/CSE/CSE313/2019-03/q5c.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q5c/ai.md) | Examined; no correction identified. |
| [q6a](../bank/CSE/CSE313/2019-03/q6a.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q6a/ai.md) | Examined; no correction identified. |
| [q6b](../bank/CSE/CSE313/2019-03/q6b.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q6b/ai.md) | Revised: Corrected all-operations-require-syscalls and fixed-thread-table claims. |
| [q6c](../bank/CSE/CSE313/2019-03/q6c.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q6c/ai.md) | Examined; no correction identified. |
| [q6d](../bank/CSE/CSE313/2019-03/q6d.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q6d/ai.md) | Revised: Qualified TLB/cache context-switch costs and same-process thread savings. |
| [q6e](../bank/CSE/CSE313/2019-03/q6e.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q6e/ai.md) | Examined; no correction identified. |
| [q7a](../bank/CSE/CSE313/2019-03/q7a.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q7a/ai.md) | Examined; no correction identified. |
| [q7b](../bank/CSE/CSE313/2019-03/q7b.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q7b/ai.md) | Examined; no correction identified. |
| [q8a](../bank/CSE/CSE313/2019-03/q8a.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q8a/ai.md) | Examined; no correction identified. |
| [q8b](../bank/CSE/CSE313/2019-03/q8b.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q8b/ai.md) | Examined; no correction identified. |
| [q8c](../bank/CSE/CSE313/2019-03/q8c.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q8c/ai.md) | Examined; no correction identified. |
| [q8d](../bank/CSE/CSE313/2019-03/q8d.md) | [AI solution](../bank/CSE/CSE313/2019-03/solutions/q8d/ai.md) | Examined; no correction identified. |

### 2021-01

| Part | Solution | Audit outcome |
|:--|:--|:--|
| [q1a](../bank/CSE/CSE313/2021-01/q1a.md) | [AI solution](../bank/CSE/CSE313/2021-01/solutions/q1a/ai.md) | Examined; no correction identified. |
| [q1b](../bank/CSE/CSE313/2021-01/q1b.md) | [AI solution](../bank/CSE/CSE313/2021-01/solutions/q1b/ai.md) | Examined; no correction identified. |
| [q1c](../bank/CSE/CSE313/2021-01/q1c.md) | [AI solution](../bank/CSE/CSE313/2021-01/solutions/q1c/ai.md) | Examined; no correction identified. |
| [q2a](../bank/CSE/CSE313/2021-01/q2a.md) | [AI solution](../bank/CSE/CSE313/2021-01/solutions/q2a/ai.md) | Revised: Separated ordinary Hamming SEC from extended SECDED error guarantees. |
| [q2b](../bank/CSE/CSE313/2021-01/q2b.md) | [AI solution](../bank/CSE/CSE313/2021-01/solutions/q2b/ai.md) | Examined; no correction identified. |
| [q2c](../bank/CSE/CSE313/2021-01/q2c.md) | [AI solution](../bank/CSE/CSE313/2021-01/solutions/q2c/ai.md) | Examined; no correction identified. |
| [q3a](../bank/CSE/CSE313/2021-01/q3a.md) | [AI solution](../bank/CSE/CSE313/2021-01/solutions/q3a/ai.md) | Revised: Replaced a counterexample missing circular wait with a multiple-instance resource example. |
| [q3b](../bank/CSE/CSE313/2021-01/q3b.md) | [AI solution](../bank/CSE/CSE313/2021-01/solutions/q3b/ai.md) | Revised: Scoped the textbook ostrich approach to application resource deadlocks. |
| [q3c](../bank/CSE/CSE313/2021-01/q3c.md) | [AI solution](../bank/CSE/CSE313/2021-01/solutions/q3c/ai.md) | Examined; no correction identified. |
| [q4a](../bank/CSE/CSE313/2021-01/q4a.md) | [AI solution](../bank/CSE/CSE313/2021-01/solutions/q4a/ai.md) | Examined; no correction identified. |
| [q4b](../bank/CSE/CSE313/2021-01/q4b.md) | [AI solution](../bank/CSE/CSE313/2021-01/solutions/q4b/ai.md) | Examined; no correction identified. |
| [q4c](../bank/CSE/CSE313/2021-01/q4c.md) | [AI solution](../bank/CSE/CSE313/2021-01/solutions/q4c/ai.md) | Revised: Qualified dynamic admission and safety-check cost rather than treating implementation assumptions as impossibilities. |
| [q5a](../bank/CSE/CSE313/2021-01/q5a.md) | [AI solution](../bank/CSE/CSE313/2021-01/solutions/q5a/ai.md) | Examined; no correction identified. |
| [q5b](../bank/CSE/CSE313/2021-01/q5b.md) | [AI solution](../bank/CSE/CSE313/2021-01/solutions/q5b/ai.md) | Examined; no correction identified. |
| [q6a](../bank/CSE/CSE313/2021-01/q6a.md) | [AI solution](../bank/CSE/CSE313/2021-01/solutions/q6a/ai.md) | Examined; no correction identified. |
| [q6b](../bank/CSE/CSE313/2021-01/q6b.md) | [AI solution](../bank/CSE/CSE313/2021-01/solutions/q6b/ai.md) | Examined; no correction identified. |
| [q6c](../bank/CSE/CSE313/2021-01/q6c.md) | [AI solution](../bank/CSE/CSE313/2021-01/solutions/q6c/ai.md) | Examined; no correction identified. |
| [q7a](../bank/CSE/CSE313/2021-01/q7a.md) | [AI solution](../bank/CSE/CSE313/2021-01/solutions/q7a/ai.md) | Examined; no correction identified. |
| [q7b](../bank/CSE/CSE313/2021-01/q7b.md) | [AI solution](../bank/CSE/CSE313/2021-01/solutions/q7b/ai.md) | Examined; no correction identified. |
| [q8a](../bank/CSE/CSE313/2021-01/q8a.md) | [AI solution](../bank/CSE/CSE313/2021-01/solutions/q8a/ai.md) | Revised: Removed APFS as a journaling example and clarified metadata-only journaling. |
| [q8b](../bank/CSE/CSE313/2021-01/q8b.md) | [AI solution](../bank/CSE/CSE313/2021-01/solutions/q8b/ai.md) | Revised: Corrected original UNIX V7 block size and distinguished a hypothetical 1 KiB configuration. |

### 2022-04

| Part | Solution | Audit outcome |
|:--|:--|:--|
| [q1a](../bank/CSE/CSE313/2022-04/q1a.md) | [AI solution](../bank/CSE/CSE313/2022-04/solutions/q1a/ai.md) | Revised: Made the general boot sequence accurate for BIOS and UEFI while preserving a compact BIOS answer. |
| [q1b](../bank/CSE/CSE313/2022-04/q1b.md) | [AI solution](../bank/CSE/CSE313/2022-04/solutions/q1b/ai.md) | Revised: Replaced ASCII process-tree art with the existing matching annotated figure. |
| [q1c](../bank/CSE/CSE313/2022-04/q1c.md) | [AI solution](../bank/CSE/CSE313/2022-04/solutions/q1c/ai.md) | Examined; no correction identified. |
| [q1d](../bank/CSE/CSE313/2022-04/q1d.md) | [AI solution](../bank/CSE/CSE313/2022-04/solutions/q1d/ai.md) | Revised: Clarified which user threads can run when a backing kernel thread blocks. |
| [q2a](../bank/CSE/CSE313/2022-04/q2a.md) | [AI solution](../bank/CSE/CSE313/2022-04/solutions/q2a/ai.md) | Examined; no correction identified. |
| [q2b](../bank/CSE/CSE313/2022-04/q2b.md) | [AI solution](../bank/CSE/CSE313/2022-04/solutions/q2b/ai.md) | Examined; no correction identified. |
| [q2c](../bank/CSE/CSE313/2022-04/q2c.md) | [AI solution](../bank/CSE/CSE313/2022-04/solutions/q2c/ai.md) | Examined; no correction identified. |
| [q2d](../bank/CSE/CSE313/2022-04/q2d.md) | [AI solution](../bank/CSE/CSE313/2022-04/solutions/q2d/ai.md) | Examined; no correction identified. |
| [q3a](../bank/CSE/CSE313/2022-04/q3a.md) | [AI solution](../bank/CSE/CSE313/2022-04/solutions/q3a/ai.md) | Revised: Labeled the hypothetical changed claim rather than presenting a guessed exam correction. |
| [q3b](../bank/CSE/CSE313/2022-04/q3b.md) | [AI solution](../bank/CSE/CSE313/2022-04/solutions/q3b/ai.md) | Examined; no correction identified. |
| [q3c](../bank/CSE/CSE313/2022-04/q3c.md) | [AI solution](../bank/CSE/CSE313/2022-04/solutions/q3c/ai.md) | Examined; no correction identified. |
| [q4a](../bank/CSE/CSE313/2022-04/q4a.md) | [AI solution](../bank/CSE/CSE313/2022-04/solutions/q4a/ai.md) | Revised: Added actual CPU/I/O timelines and corrected the cause of an I/O-bound trend. |
| [q4b](../bank/CSE/CSE313/2022-04/q4b.md) | [AI solution](../bank/CSE/CSE313/2022-04/solutions/q4b/ai.md) | Examined; no correction identified. |
| [q4c](../bank/CSE/CSE313/2022-04/q4c.md) | [AI solution](../bank/CSE/CSE313/2022-04/solutions/q4c/ai.md) | Examined; no correction identified. |
| [q5](../bank/CSE/CSE313/2022-04/q5.md) | Shared setup | Examined with its child parts; no standalone answer required. |
| [q5a](../bank/CSE/CSE313/2022-04/q5a.md) | [AI solution](../bank/CSE/CSE313/2022-04/solutions/q5a/ai.md) | Examined; no correction identified. |
| [q5b](../bank/CSE/CSE313/2022-04/q5b.md) | [AI solution](../bank/CSE/CSE313/2022-04/solutions/q5b/ai.md) | Revised: Qualified memory-mapped device instruction/ordering restrictions. |
| [q5c](../bank/CSE/CSE313/2022-04/q5c.md) | [AI solution](../bank/CSE/CSE313/2022-04/solutions/q5c/ai.md) | Examined; no correction identified. |
| [q6a](../bank/CSE/CSE313/2022-04/q6a.md) | [AI solution](../bank/CSE/CSE313/2022-04/solutions/q6a/ai.md) | Examined; no correction identified. |
| [q6b](../bank/CSE/CSE313/2022-04/q6b.md) | [AI solution](../bank/CSE/CSE313/2022-04/solutions/q6b/ai.md) | Examined; no correction identified. |
| [q6c](../bank/CSE/CSE313/2022-04/q6c.md) | [AI solution](../bank/CSE/CSE313/2022-04/solutions/q6c/ai.md) | Examined; no correction identified. |
| [q7a](../bank/CSE/CSE313/2022-04/q7a.md) | [AI solution](../bank/CSE/CSE313/2022-04/solutions/q7a/ai.md) | Examined; no correction identified. |
| [q7b](../bank/CSE/CSE313/2022-04/q7b.md) | [AI solution](../bank/CSE/CSE313/2022-04/solutions/q7b/ai.md) | Examined; no correction identified. |
| [q7c](../bank/CSE/CSE313/2022-04/q7c.md) | [AI solution](../bank/CSE/CSE313/2022-04/solutions/q7c/ai.md) | Examined; no correction identified. |
| [q8a](../bank/CSE/CSE313/2022-04/q8a.md) | [AI solution](../bank/CSE/CSE313/2022-04/solutions/q8a/ai.md) | Examined; no correction identified. |
| [q8b](../bank/CSE/CSE313/2022-04/q8b.md) | [AI solution](../bank/CSE/CSE313/2022-04/solutions/q8b/ai.md) | Revised: Removed false claim that DMA is generally slower and distinguished PIO from interrupt completion. |
| [q8c](../bank/CSE/CSE313/2022-04/q8c.md) | [AI solution](../bank/CSE/CSE313/2022-04/solutions/q8c/ai.md) | Examined; no correction identified. |

### 2023-03

| Part | Solution | Audit outcome |
|:--|:--|:--|
| [q1a](../bank/CSE/CSE313/2023-03/q1a.md) | [AI solution](../bank/CSE/CSE313/2023-03/solutions/q1a/ai.md) | Examined; no correction identified. |
| [q1b](../bank/CSE/CSE313/2023-03/q1b.md) | [AI solution](../bank/CSE/CSE313/2023-03/solutions/q1b/ai.md) | Revised: Clarified blocking queue-lock interpretation, preemption assumptions, and overhead limits. |
| [q1c](../bank/CSE/CSE313/2023-03/q1c.md) | [AI solution](../bank/CSE/CSE313/2023-03/solutions/q1c/ai.md) | Revised: Made ticket-lock memory-order and counter assumptions explicit. Explained atomic primitive adaptation and modular ticket uniqueness. Made the ticket walkthrough match unsigned modular counters. |
| [q2a](../bank/CSE/CSE313/2023-03/q2a.md) | [AI solution](../bank/CSE/CSE313/2023-03/solutions/q2a/ai.md) | Examined; no correction identified. |
| [q2b](../bank/CSE/CSE313/2023-03/q2b.md) | [AI solution](../bank/CSE/CSE313/2023-03/solutions/q2b/ai.md) | Examined; no correction identified. |
| [q2c](../bank/CSE/CSE313/2023-03/q2c.md) | [AI solution](../bank/CSE/CSE313/2023-03/solutions/q2c/ai.md) | Revised: Stated intended waitpid and unbuffered-output assumptions without altering the question. |
| [q3a](../bank/CSE/CSE313/2023-03/q3a.md) | [AI solution](../bank/CSE/CSE313/2023-03/solutions/q3a/ai.md) | Revised: Replaced volatile shared flag with an atomic flag and explained the synchronization guarantee. |
| [q3b](../bank/CSE/CSE313/2023-03/q3b.md) | [AI solution](../bank/CSE/CSE313/2023-03/solutions/q3b/ai.md) | Examined; no correction identified. |
| [q3c](../bank/CSE/CSE313/2023-03/q3c.md) | [AI solution](../bank/CSE/CSE313/2023-03/solutions/q3c/ai.md) | Examined; no correction identified. |
| [q4a](../bank/CSE/CSE313/2023-03/q4a.md) | [AI solution](../bank/CSE/CSE313/2023-03/solutions/q4a/ai.md) | Examined; no correction identified. |
| [q4b](../bank/CSE/CSE313/2023-03/q4b.md) | [AI solution](../bank/CSE/CSE313/2023-03/solutions/q4b/ai.md) | Examined; no correction identified. |
| [q4c](../bank/CSE/CSE313/2023-03/q4c.md) | [AI solution](../bank/CSE/CSE313/2023-03/solutions/q4c/ai.md) | Revised: Qualified response and waiting bounds for RR with arrivals. |
| [q5a](../bank/CSE/CSE313/2023-03/q5a.md) | [AI solution](../bank/CSE/CSE313/2023-03/solutions/q5a/ai.md) | Revised: Removed an invented extra seek and separated placement assumptions from the inode capacity. |
| [q5b](../bank/CSE/CSE313/2023-03/q5b.md) | [AI solution](../bank/CSE/CSE313/2023-03/solutions/q5b/ai.md) | Revised: Clarified TLB membership and valid versus present PTE checks. |
| [q5c](../bank/CSE/CSE313/2023-03/q5c.md) | [AI solution](../bank/CSE/CSE313/2023-03/solutions/q5c/ai.md) | Revised: Qualified retry semantics and duplicate suppression for non-idempotent RPCs. |
| [q6a](../bank/CSE/CSE313/2023-03/q6a.md) | [AI solution](../bank/CSE/CSE313/2023-03/solutions/q6a/ai.md) | Revised: Stopped seek accounting at the final serviced request of each batch and stated timing assumptions. |
| [q6b](../bank/CSE/CSE313/2023-03/q6b.md) | [AI solution](../bank/CSE/CSE313/2023-03/solutions/q6b/ai.md) | Examined; no correction identified. |
| [q6c](../bank/CSE/CSE313/2023-03/q6c.md) | [AI solution](../bank/CSE/CSE313/2023-03/solutions/q6c/ai.md) | Examined; no correction identified. |
| [q7a](../bank/CSE/CSE313/2023-03/q7a.md) | [AI solution](../bank/CSE/CSE313/2023-03/solutions/q7a/ai.md) | Examined; no correction identified. |
| [q7b](../bank/CSE/CSE313/2023-03/q7b.md) | [AI solution](../bank/CSE/CSE313/2023-03/solutions/q7b/ai.md) | Revised: Kept TxE and durable data-before-commit ordering in checksum metadata journaling. |
| [q7c](../bank/CSE/CSE313/2023-03/q7c.md) | [AI solution](../bank/CSE/CSE313/2023-03/solutions/q7c/ai.md) | Examined; no correction identified. |
| [q7d](../bank/CSE/CSE313/2023-03/q7d.md) | [AI solution](../bank/CSE/CSE313/2023-03/solutions/q7d/ai.md) | Revised: Clarified disk-count convention and the limited reliability benefit of mirrored parity. |
| [q8a](../bank/CSE/CSE313/2023-03/q8a.md) | [AI solution](../bank/CSE/CSE313/2023-03/solutions/q8a/ai.md) | Revised: Made rename crash consistency depend on recovery protocol rather than append-only writes alone. |
| [q8b](../bank/CSE/CSE313/2023-03/q8b.md) | [AI solution](../bank/CSE/CSE313/2023-03/solutions/q8b/ai.md) | Revised: Prevented a lost wakeup in DMA completion pseudocode and required mapped/pinned buffers. |
| [q8c](../bank/CSE/CSE313/2023-03/q8c.md) | [AI solution](../bank/CSE/CSE313/2023-03/solutions/q8c/ai.md) | Examined; no correction identified. |

### 2023-10

| Part | Solution | Audit outcome |
|:--|:--|:--|
| [q1a](../bank/CSE/CSE313/2023-10/q1a.md) | [AI solution](../bank/CSE/CSE313/2023-10/solutions/q1a/ai.md) | Examined; no correction identified. |
| [q1b](../bank/CSE/CSE313/2023-10/q1b.md) | [AI solution](../bank/CSE/CSE313/2023-10/solutions/q1b/ai.md) | Examined; no correction identified. |
| [q1c](../bank/CSE/CSE313/2023-10/q1c.md) | [AI solution](../bank/CSE/CSE313/2023-10/solutions/q1c/ai.md) | Examined; no correction identified. |
| [q2a](../bank/CSE/CSE313/2023-10/q2a.md) | [AI solution](../bank/CSE/CSE313/2023-10/solutions/q2a/ai.md) | Examined; no correction identified. |
| [q2b](../bank/CSE/CSE313/2023-10/q2b.md) | [AI solution](../bank/CSE/CSE313/2023-10/solutions/q2b/ai.md) | Revised: Defined fork numbering and supplied a complete deadlock-free argument for the asymmetric protocol. |
| [q2c](../bank/CSE/CSE313/2023-10/q2c.md) | [AI solution](../bank/CSE/CSE313/2023-10/solutions/q2c/ai.md) | Examined; no correction identified. |
| [q2d](../bank/CSE/CSE313/2023-10/q2d.md) | [AI solution](../bank/CSE/CSE313/2023-10/solutions/q2d/ai.md) | Examined; no correction identified. |
| [q3a](../bank/CSE/CSE313/2023-10/q3a.md) | [AI solution](../bank/CSE/CSE313/2023-10/solutions/q3a/ai.md) | Examined; no correction identified. |
| [q3b](../bank/CSE/CSE313/2023-10/q3b.md) | [AI solution](../bank/CSE/CSE313/2023-10/solutions/q3b/ai.md) | Examined; no correction identified. |
| [q3c](../bank/CSE/CSE313/2023-10/q3c.md) | [AI solution](../bank/CSE/CSE313/2023-10/solutions/q3c/ai.md) | Examined; no correction identified. |
| [q4a](../bank/CSE/CSE313/2023-10/q4a.md) | [AI solution](../bank/CSE/CSE313/2023-10/solutions/q4a/ai.md) | Examined; no correction identified. |
| [q4b](../bank/CSE/CSE313/2023-10/q4b.md) | [AI solution](../bank/CSE/CSE313/2023-10/solutions/q4b/ai.md) | Revised: Corrected cache/TLB flush claims and clarified thread-local variables. |
| [q4c](../bank/CSE/CSE313/2023-10/q4c.md) | [AI solution](../bank/CSE/CSE313/2023-10/solutions/q4c/ai.md) | Revised: Added actual scheduler/table diagrams and stated the thread-mapping assumptions. |
| [q5a](../bank/CSE/CSE313/2023-10/q5a.md) | [AI solution](../bank/CSE/CSE313/2023-10/solutions/q5a/ai.md) | Revised: Used coherent TLB invalidation as the main model and added the actual Clock/TLB trace. |
| [q5b](../bank/CSE/CSE313/2023-10/q5b.md) | [AI solution](../bank/CSE/CSE313/2023-10/solutions/q5b/ai.md) | Revised: Avoided presenting FFS as read-only optimization. |
| [q6a](../bank/CSE/CSE313/2023-10/q6a.md) | [AI solution](../bank/CSE/CSE313/2023-10/solutions/q6a/ai.md) | Revised: Added safe buffer ownership, all requested driver pseudocodes, and the missing C/output assumptions. |
| [q6b](../bank/CSE/CSE313/2023-10/q6b.md) | [AI solution](../bank/CSE/CSE313/2023-10/solutions/q6b/ai.md) | Examined; no correction identified. |
| [q7a-i](../bank/CSE/CSE313/2023-10/q7a-i.md) | [AI solution](../bank/CSE/CSE313/2023-10/solutions/q7a-i/ai.md) | Examined; no correction identified. |
| [q7a-ii](../bank/CSE/CSE313/2023-10/q7a-ii.md) | [AI solution](../bank/CSE/CSE313/2023-10/solutions/q7a-ii/ai.md) | Revised: Stated binary-unit and initial-positioning assumptions for the FFS read-time estimate. |
| [q7a](../bank/CSE/CSE313/2023-10/q7a.md) | Shared setup | Examined with its child parts; no standalone answer required. |
| [q7b](../bank/CSE/CSE313/2023-10/q7b.md) | [AI solution](../bank/CSE/CSE313/2023-10/solutions/q7b/ai.md) | Examined; no correction identified. |
| [q8a-i](../bank/CSE/CSE313/2023-10/q8a-i.md) | [AI solution](../bank/CSE/CSE313/2023-10/solutions/q8a-i/ai.md) | Examined; no correction identified. |
| [q8a-ii](../bank/CSE/CSE313/2023-10/q8a-ii.md) | [AI solution](../bank/CSE/CSE313/2023-10/solutions/q8a-ii/ai.md) | Revised: Preserved hard-link and open-descriptor semantics during LFS unlink. |
| [q8a-iii](../bank/CSE/CSE313/2023-10/q8a-iii.md) | [AI solution](../bank/CSE/CSE313/2023-10/solutions/q8a-iii/ai.md) | Revised: Removed unsupported claim that append-only segment writes automatically make unlink atomic. |
| [q8a-iv](../bank/CSE/CSE313/2023-10/q8a-iv.md) | [AI solution](../bank/CSE/CSE313/2023-10/solutions/q8a-iv/ai.md) | Revised: Included rotational latency and explicit stripe/seek/unit assumptions in RAID throughput estimates. |
| [q8a](../bank/CSE/CSE313/2023-10/q8a.md) | Shared setup | Examined with its child parts; no standalone answer required. |
| [q8b](../bank/CSE/CSE313/2023-10/q8b.md) | [AI solution](../bank/CSE/CSE313/2023-10/solutions/q8b/ai.md) | Examined; no correction identified. |

### 2025-01

| Part | Solution | Audit outcome |
|:--|:--|:--|
| [q1a](../bank/CSE/CSE313/2025-01/q1a.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q1a/ai.md) | Examined; no correction identified. |
| [q1b](../bank/CSE/CSE313/2025-01/q1b.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q1b/ai.md) | Examined; no correction identified. |
| [q1c](../bank/CSE/CSE313/2025-01/q1c.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q1c/ai.md) | Revised: Corrected unconditional TLB/cache flush claims and explained same-process thread switching. |
| [q2a](../bank/CSE/CSE313/2025-01/q2a.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q2a/ai.md) | Examined; no correction identified. |
| [q2b](../bank/CSE/CSE313/2025-01/q2b.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q2b/ai.md) | Examined; no correction identified. |
| [q2c](../bank/CSE/CSE313/2025-01/q2c.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q2c/ai.md) | Revised: Corrected all-operations-require-syscalls and fixed-thread-table claims. |
| [q3a](../bank/CSE/CSE313/2025-01/q3a.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q3a/ai.md) | Examined; no correction identified. |
| [q3b](../bank/CSE/CSE313/2025-01/q3b.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q3b/ai.md) | Examined; no correction identified. |
| [q3c](../bank/CSE/CSE313/2025-01/q3c.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q3c/ai.md) | Examined; no correction identified. |
| [q4a](../bank/CSE/CSE313/2025-01/q4a.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q4a/ai.md) | Revised: Added the requested life-cycle figure and distinguished CPU starvation from resource starvation. |
| [q4b](../bank/CSE/CSE313/2025-01/q4b.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q4b/ai.md) | Revised: Replaced ASCII process-tree art with the existing matching annotated figure. |
| [q4c](../bank/CSE/CSE313/2025-01/q4c.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q4c/ai.md) | Examined; no correction identified. |
| [q5a](../bank/CSE/CSE313/2025-01/q5a.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q5a/ai.md) | Revised: Answered total physical memory as the primary result and separated page-table overhead. |
| [q5b](../bank/CSE/CSE313/2025-01/q5b.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q5b/ai.md) | Revised: Added the requested derivation, defined effective-rate fraction, and checked units. |
| [q5c](../bank/CSE/CSE313/2025-01/q5c.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q5c/ai.md) | Revised: Corrected exact-LRU list complexity and explained safe dirty-page completion. Added the requested Clock-hand illustration. |
| [q6a](../bank/CSE/CSE313/2025-01/q6a.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q6a/ai.md) | Revised: Corrected comparison with single-parity RAID-4 capacity and throughput. Made the surviving two-disk failure combinations explicitly cross-group. |
| [q6b](../bank/CSE/CSE313/2025-01/q6b.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q6b/ai.md) | Examined; no correction identified. |
| [q6c](../bank/CSE/CSE313/2025-01/q6c.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q6c/ai.md) | Revised: Separated valid mapping from physical residency and qualified clean-page eviction. |
| [q6d](../bank/CSE/CSE313/2025-01/q6d.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q6d/ai.md) | Revised: Clarified page-fault causes and avoided claiming all faults need disk I/O. |
| [q6e](../bank/CSE/CSE313/2025-01/q6e.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q6e/ai.md) | Examined; no correction identified. |
| [q7a](../bank/CSE/CSE313/2025-01/q7a.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q7a/ai.md) | Examined; no correction identified. |
| [q7b](../bank/CSE/CSE313/2025-01/q7b.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q7b/ai.md) | Examined; no correction identified. |
| [q7c](../bank/CSE/CSE313/2025-01/q7c.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q7c/ai.md) | Examined; no correction identified. |
| [q7d](../bank/CSE/CSE313/2025-01/q7d.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q7d/ai.md) | Examined; no correction identified. |
| [q8a](../bank/CSE/CSE313/2025-01/q8a.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q8a/ai.md) | Examined; no correction identified. |
| [q8b](../bank/CSE/CSE313/2025-01/q8b.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q8b/ai.md) | Examined; no correction identified. |
| [q8c](../bank/CSE/CSE313/2025-01/q8c.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q8c/ai.md) | Examined; no correction identified. |
| [q8d](../bank/CSE/CSE313/2025-01/q8d.md) | [AI solution](../bank/CSE/CSE313/2025-01/solutions/q8d/ai.md) | Revised: Completed the on-disk imap/checkpoint update chain instead of stopping at the in-memory map. |

### 2025-09

| Part | Solution | Audit outcome |
|:--|:--|:--|
| [q1a](../bank/CSE/CSE313/2025-09/q1a.md) | [AI solution](../bank/CSE/CSE313/2025-09/solutions/q1a/ai.md) | Examined; no correction identified. |
| [q1b](../bank/CSE/CSE313/2025-09/q1b.md) | [AI solution](../bank/CSE/CSE313/2025-09/solutions/q1b/ai.md) | Revised: Added actual scheduler/table diagrams and stated the thread-mapping assumptions. |
| [q1c](../bank/CSE/CSE313/2025-09/q1c.md) | [AI solution](../bank/CSE/CSE313/2025-09/solutions/q1c/ai.md) | Examined; no correction identified. |
| [q2a](../bank/CSE/CSE313/2025-09/q2a.md) | [AI solution](../bank/CSE/CSE313/2025-09/solutions/q2a/ai.md) | Examined; no correction identified. |
| [q2b](../bank/CSE/CSE313/2025-09/q2b.md) | [AI solution](../bank/CSE/CSE313/2025-09/solutions/q2b/ai.md) | Examined; no correction identified. |
| [q2c](../bank/CSE/CSE313/2025-09/q2c.md) | [AI solution](../bank/CSE/CSE313/2025-09/solutions/q2c/ai.md) | Examined; no correction identified. |
| [q2d](../bank/CSE/CSE313/2025-09/q2d.md) | [AI solution](../bank/CSE/CSE313/2025-09/solutions/q2d/ai.md) | Examined; no correction identified. |
| [q3a](../bank/CSE/CSE313/2025-09/q3a.md) | [AI solution](../bank/CSE/CSE313/2025-09/solutions/q3a/ai.md) | Revised: Added request validation and distinguished simulated Work from real Available. |
| [q3b](../bank/CSE/CSE313/2025-09/q3b.md) | [AI solution](../bank/CSE/CSE313/2025-09/solutions/q3b/ai.md) | Revised: Separated CPU privilege from address-space division and removed syscall-only kernel-entry claim. |
| [q3c](../bank/CSE/CSE313/2025-09/q3c.md) | [AI solution](../bank/CSE/CSE313/2025-09/solutions/q3c/ai.md) | Revised: Corrected unconditional TLB/cache flush claims and explained same-process thread switching. |
| [q3d](../bank/CSE/CSE313/2025-09/q3d.md) | [AI solution](../bank/CSE/CSE313/2025-09/solutions/q3d/ai.md) | Revised: Made the general boot sequence accurate for BIOS and UEFI while preserving a compact BIOS answer. |
| [q4a](../bank/CSE/CSE313/2025-09/q4a.md) | [AI solution](../bank/CSE/CSE313/2025-09/solutions/q4a/ai.md) | Examined; no correction identified. |
| [q4b](../bank/CSE/CSE313/2025-09/q4b.md) | [AI solution](../bank/CSE/CSE313/2025-09/solutions/q4b/ai.md) | Examined; no correction identified. |
| [q4c](../bank/CSE/CSE313/2025-09/q4c.md) | [AI solution](../bank/CSE/CSE313/2025-09/solutions/q4c/ai.md) | Examined; no correction identified. |
| [q5a](../bank/CSE/CSE313/2025-09/q5a.md) | [AI solution](../bank/CSE/CSE313/2025-09/solutions/q5a/ai.md) | Revised: Included unused root entries and distinguished packed table sizes from page-sized allocations. |
| [q5b](../bank/CSE/CSE313/2025-09/q5b.md) | [AI solution](../bank/CSE/CSE313/2025-09/solutions/q5b/ai.md) | Revised: Added the requested derivation, defined effective-rate fraction, and checked units. |
| [q5c](../bank/CSE/CSE313/2025-09/q5c.md) | [AI solution](../bank/CSE/CSE313/2025-09/solutions/q5c/ai.md) | Revised: Protected dirty-page write-back against concurrent writes and avoided spinning when all candidates are pending. |
| [q6a](../bank/CSE/CSE313/2025-09/q6a.md) | [AI solution](../bank/CSE/CSE313/2025-09/solutions/q6a/ai.md) | Examined; no correction identified. |
| [q6b](../bank/CSE/CSE313/2025-09/q6b.md) | [AI solution](../bank/CSE/CSE313/2025-09/solutions/q6b/ai.md) | Revised: Distinguished limited reliability improvement from guaranteed two-disk tolerance. |
| [q6c](../bank/CSE/CSE313/2025-09/q6c.md) | [AI solution](../bank/CSE/CSE313/2025-09/solutions/q6c/ai.md) | Revised: Added a segment diagram, corrected inode-number indirection, and distinguished writes from file creation. Defined the segment-summary abbreviation used in the new diagram. |
| [q7a](../bank/CSE/CSE313/2025-09/q7a.md) | [AI solution](../bank/CSE/CSE313/2025-09/solutions/q7a/ai.md) | Examined; no correction identified. |
| [q7b](../bank/CSE/CSE313/2025-09/q7b.md) | [AI solution](../bank/CSE/CSE313/2025-09/solutions/q7b/ai.md) | Revised: Separated ordered/writeback modes, existing-block overwrites, and committed from incomplete data journals. |
| [q7c](../bank/CSE/CSE313/2025-09/q7c.md) | [AI solution](../bank/CSE/CSE313/2025-09/solutions/q7c/ai.md) | Revised: Qualified checksum correctness, retained data ordering, and removed an unsafe external-data-checksum workaround. |
| [q8a](../bank/CSE/CSE313/2025-09/q8a.md) | [AI solution](../bank/CSE/CSE313/2025-09/solutions/q8a/ai.md) | Revised: Added an allocation diagram and separated external fragmentation from power-of-two rounding. Made the external-fragmentation measurement explicit instead of attributing buddy rounding to holes. |
| [q8b](../bank/CSE/CSE313/2025-09/q8b.md) | [AI solution](../bank/CSE/CSE313/2025-09/solutions/q8b/ai.md) | Examined; no correction identified. |
| [q8c](../bank/CSE/CSE313/2025-09/q8c.md) | [AI solution](../bank/CSE/CSE313/2025-09/solutions/q8c/ai.md) | Revised: Explained validation of checkpoint copies and limits of segment-level recovery. |
