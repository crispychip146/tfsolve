---
author: ai
via: chat
status: unverified
imported_from: cse313_mem_fs_solved.tex
changes:
  - "2026-10-31: Separated valid mapping from physical residency and qualified clean-page eviction."
---
A page-table entry connects a virtual page to a physical frame and records whether an access is permitted. These three bits answer different questions:

| Bit | Meaning | OS use |
|:--|:--|:--|
| Present | The page currently resides in a physical frame | If absent, trap so the OS can bring it in or reject an invalid access |
| Dirty/modified | The resident page has been written since the OS last marked it clean | Preserve changes before reusing the frame |
| Reference/accessed | The page has been read or written since the OS last cleared the bit | Approximate recent use, as in Clock replacement |

**Validity and presence are distinct concepts.** A valid virtual page can be temporarily absent because it was swapped out; an invalid address is outside the process's mappings. Some hardware encodes both through a non-present PTE and lets the OS distinguish them using additional mapping information.

A dirty page normally needs write-back before eviction. A clean page can be discarded if a recoverable backing copy exists, or its contents can otherwise be reconstructed, for example a fresh zero-filled anonymous page. Clearing an accessed or dirty bit must follow the architecture's TLB rules so future accesses are observed correctly.
