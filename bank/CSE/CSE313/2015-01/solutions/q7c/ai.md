---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "Blocking waits for progress; nonblocking returns immediately with available progress or EAGAIN/EWOULDBLOCK."
sources: ["Tanenbaum MOS 4e, sec. 2.4.1 (fig. 2-39: bursts of CPU usage alternate with periods of waiting for I/O)"]
changes:
  - "2026-10-31: Replaced unrelated CPU/I/O-bound notes with blocking versus nonblocking syscall semantics."
---
A system call may need an event that has not happened yet: for example, `read()` on an empty pipe needs a writer to supply bytes. The distinction is what the OS does while that event is unavailable.

| Situation | Blocking call | Nonblocking call |
|:--|:--|:--|
| Requested progress is possible now | Return the available result | Return the available result |
| No progress is possible now | Suspend the calling thread until progress, an error or interruption | Return immediately, commonly with EAGAIN/EWOULDBLOCK |
| CPU while waiting | Available to another runnable task | Caller must decide what else to do |
| Typical use | Simple sequential code | Event loops serving many connections |

For example, a blocking pipe read sleeps if the pipe is empty while a writer still exists. With `O_NONBLOCK`, the same read returns `-1` with `errno` set to `EAGAIN` or `EWOULDBLOCK`. If all writers have closed the pipe, an empty read returns **0 for end-of-file**, rather than waiting or reporting temporary unavailability.

Both kinds can return **partial transfers**: blocking does not promise to fill an arbitrary buffer. Nonblocking also differs from asynchronous I/O: it reports the result of the present attempt, while asynchronous I/O submits work whose completion is reported later. Repeatedly retrying a nonblocking call in a tight loop wastes CPU; use readiness notification such as `poll()` before retrying when appropriate.
