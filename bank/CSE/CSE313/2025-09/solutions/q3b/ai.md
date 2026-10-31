---
author: ai
via: chat
status: unverified
summary: "Mode is CPU privilege; space is address-space ownership/protection. Syscalls, exceptions and interrupts can enter the kernel."
sources: [Introduction slides 12-16, 'Tanenbaum, MOS 4e, sec. 1.3.3']
imported_from: tfsolve-questions/data/solutions/2023-24.json
changes:
  - "2026-10-31: Separated CPU privilege from address-space division and removed syscall-only kernel-entry claim."
---
**(i) User mode and kernel mode are CPU privilege levels.** User-mode code cannot execute privileged instructions such as changing page-table roots or disabling interrupts. To request an OS service, it uses a controlled system-call entry. Hardware interrupts and exceptions can also transfer control to privileged kernel handlers. Kernel privilege permits OS operations, but does not mean every address is mapped or every architectural protection check disappears.

**(ii) User space and kernel space describe protected address-space regions.** User space contains an application's code, heap and stacks. Kernel space contains OS code and data protected from ordinary application access. A kernel handler can inspect a user buffer after validating it; a process cannot gain kernel access merely by constructing a kernel-space pointer.

A system call illustrates both ideas: the CPU enters kernel **mode**, runs kernel code using protected mappings, checks the supplied user-space pointer, performs the operation and returns to user mode. Entering kernel mode is not the same as switching to a different process.
