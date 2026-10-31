---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "The driver programs the DMA engine (address, count, direction), the CPU runs other work, and the DMA controller interrupts when the transfer ends."
sources: ["OSTEP ch. 36 (DMA)"]
changes:
  - "2026-10-31: Prevented a lost wakeup in DMA completion pseudocode and required mapped/pinned buffers."
---
DMA moves the bytes between a device and RAM. The CPU's job is to prepare a buffer, start the transfer, then handle completion; it does not copy each byte through device registers.

![Timeline of a DMA transfer](figures/dma.png)

The difficult case is a fast completion: the device may interrupt immediately after `START`. A bare `start(); sleep();` can lose that wakeup. Use a completion predicate protected by a lock and a condition wait that **atomically releases the lock and sleeps**.

```text
dma_write(buffer, length):
    mapping = pin_and_dma_map(buffer, length)
    lock(device_lock)
    while device_busy:
        wait(device_available, device_lock)
    device_busy = true
    done = false
    program_DMA(mapping, length, TO_DEVICE)
    start_device()
    while not done:
        wait(completion, device_lock)
    status = completion_status
    device_busy = false
    signal(device_available)
    unlock(device_lock)
    dma_unmap_and_unpin(mapping)
    return status

completion_interrupt():
    lock_interrupt_safe(device_lock)
    completion_status = read_device_status()
    acknowledge_interrupt()
    done = true
    signal(completion)
    unlock_interrupt_safe(device_lock)
```

This is conceptual kernel pseudocode: the lock/wait implementation must support interrupt context without sleeping in the interrupt handler. For example, acquiring the shared spinlock from process context disables the local completion interrupt, and the wait prepares the sleeper before releasing that lock. A kernel completion primitive packages those details.

The OS gives the device a **DMA address**, which may be an IOMMU address. Pinning prevents the backing frames from moving during the transfer; DMA mapping handles device addressing, scatter/gather and cache-coherency requirements. Once started, the device transfers bytes while other tasks use the CPU, and its completion interrupt releases the waiter.
