---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "sleep: raise the processor level, mark the process asleep on the address, put it on the sleep hash queue, save the sleep address and priority, then switch context; on wakeup restore priority and (if interruptible) check for signals."
sources: ["Bach, ch. 6 (algorithm sleep)"]
changes:
  - "2026-10-31: Clarified kernel sleep priority versus processor interrupt level and user sleep API."
---
```text
algorithm sleep
input:  (1) sleep address
        (2) priority
output: 1 if process awakened as a result of a signal that process catches,
        longjmp algorithm if process awakened as a result of a signal it does not catch,
        0 otherwise
{
    raise processor execution level to block all interrupts;
    set process state to sleep;
    put process on sleep hash queue, based on sleep address;
    save sleep address in process table slot;
    set process priority level to input priority;
    if (process sleep is NOT interruptible)
    {
        do context switch;
        /* process resumes execution here when it wakes up */
        reset processor priority level to allow interrupts as when process went to sleep;
        return (0);
    }
    /* here, process sleep is interruptible by signals */
    if (no signal pending against process)
    {
        do context switch;
        if (no signal pending against process)
        {
            reset processor priority level to what it was when process went to sleep;
            return (0);
        }
    }
    remove process from sleep hash queue, if still there;
    reset processor priority level to what it was when process went to sleep;
    if (process catches signal) return (1);
    do longjmp algorithm;
}
```

**Explanation.** This is the historical kernel sleep-on-an-event routine, not the user-space timed `sleep(seconds)` API. A process calls `sleep` when it must wait for an event (e.g. a locked buffer or inode, I/O completion). The kernel (1) blocks interrupts so the update is atomic; (2) sets the state to *asleep*, puts the process on the **sleep hash queue** chosen by the sleep address and records the address and priority in the process table; (3) performs a **context switch** to run another process. When the event occurs, `wakeup(address)` makes the process ready; when it is scheduled it continues after the context switch, restores the processor level and returns. The kernel sleep-priority argument determines whether signals may interrupt the wait (traditionally high scheduling priority, represented by a low numeric value, is non-interruptible). This differs from the processor interrupt-execution level, which is raised only to protect the transition into sleep: a signal removes the process from the queue and `sleep` returns 1 or does a `longjmp`.
