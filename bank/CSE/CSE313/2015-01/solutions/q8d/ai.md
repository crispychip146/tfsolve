---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "One semaphore per fork; philosopher i always takes fork i, so all five can eat independently."
sources: ["Tanenbaum MOS 4e, sec. 2.5.1 (dining philosophers), 2.3.7 (monitors)"]
changes:
  - "2026-10-31: Used the one-fork requirement to achieve the full five-philosopher parallelism directly."
---
The classical problem needs **two** forks per philosopher; this variation needs only **one**. Number philosophers $0\ldots4$ and let fork $i$ be the fork on philosopher $i$'s left. Each fork is physically shared by two neighbours, but we may choose a protocol in which every philosopher consistently chooses the left one.

```c
#define N 5
semaphore fork[N] = {1, 1, 1, 1, 1};

void philosopher(int i) {
    while (TRUE) {
        think();
        down(&fork[i]);
        eat();
        up(&fork[i]);
    }
}
```

**Why this achieves maximum parallelism.** The assignment $i\mapsto\text{fork }i$ is one-to-one: no other philosopher requests that fork under this protocol. Thus if all five are hungry, all five obtain distinct forks and eat together. Five is also an upper bound, because there are only five philosophers and five forks, so the protocol reaches the maximum.

**Safety and progress.** No fork is used by two philosophers; no philosopher holds one resource while requesting another, so no deadlock forms. With eventual CPU scheduling and finite `eat()` calls, every hungry philosopher progresses. This solution relies on the stated permission to eat with either one fork; it would fail immediately for the classical two-fork problem. Reserving one side by protocol also avoids a greedy arbitrary-choice allocation leaving an unused fork that a blocked philosopher cannot reach.
