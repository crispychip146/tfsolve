---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "DMA reduces CPU copying; setup/pinning and memory-bus contention are costs. Device-independent I/O provides common interfaces, buffering and error handling."
sources: ["Tanenbaum MOS 4e, sec. 5.1.4 (DMA) and sec. 5.3.2 (device-independent I/O software)"]
changes:
  - "2026-10-31: Removed false claim that DMA is generally slower and distinguished PIO from interrupt completion."
---
**DMA.**

- **Major advantage:** the CPU is not involved in the transfer. With programmed I/O, the CPU moves the data itself, using either polling or interrupt-driven service. With DMA it programs the controller (address, count, direction) and normally receives a completion interrupt per transfer, so it can run other processes meanwhile.
- **Major disadvantage:** DMA needs setup, buffer mapping/pinning and synchronization, and competes with the CPU for memory/interconnect bandwidth (*cycle stealing*). For a tiny transfer, setup can cost more than a direct CPU copy. DMA is not inherently slower than the CPU: it is designed to sustain device transfers while freeing CPU time.

**Functions of device-independent I/O software** (so that applications and drivers are written once, independent of the individual device):

1. **Uniform interfacing for device drivers:** every driver presents the same functions (read, write, ...) to the kernel, so a new driver can be added without changing the OS; also naming of devices and protection (who may access which device).
2. **Buffering:** data is buffered in kernel space when the user buffer is not yet available or when the device delivers data in different unit sizes, to decouple the device speed from the process.
3. **Error reporting:** most errors are handled by the drivers, but programming errors (e.g. writing to an input device) and persistent device errors are reported to the caller by this layer.
4. **Allocating and releasing dedicated devices:** devices that cannot be shared (e.g. a CD recorder) are allocated to one process at a time, with open/close or queued requests.
5. **Device-independent block size:** disks may have different sector sizes, but this layer presents a **uniform logical block size** to the upper layers.
