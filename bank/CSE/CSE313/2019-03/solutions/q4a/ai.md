---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "Programmed I/O polls the printer status per character, interrupt-driven I/O sends a character per interrupt, DMA hands the whole string to the DMA controller and gets one interrupt at the end."
sources: ["Tanenbaum MOS 4e, sec. 5.2.2-5.2.4 (programmed I/O, interrupt-driven I/O, I/O using DMA)"]
changes:
  - "2026-10-31: Initialized interrupt-transfer state correctly and used a completion semaphore to avoid lost wakeups."
---
Let the first name be the illustrative string `"Rahim"`. Assume a serialized printer driver owns a kernel buffer for the duration of the transfer; the device raises an interrupt when it is ready for the next character or when the final transfer completes.

**(i) Programmed I/O: the CPU copies each character and polls readiness.**

```text
copy_from_user(buffer, name, length)
for i = 0 .. length-1:
    while not printer_ready():
        poll
    write_printer_data(buffer[i])
wait_until_final_character_completes()
```

The CPU is occupied with checking and copying instead of doing other work. Polling is the completion strategy; programmed I/O means the CPU moves the bytes.

**(ii) Interrupt-driven programmed I/O: the CPU still copies characters, but can sleep between them.**

```text
start_print(name, length):
    copy_from_user(buffer, name, length)
    completed = semaphore(initial=0)
    next = 0
    remaining = length
    if length == 0: return
    install_transfer_state_while_printer_interrupt_is_masked()
    wait_until_printer_ready()
    write_printer_data(buffer[next])
    next += 1
    remaining -= 1
    enable_printer_interrupt()
    down(completed)

printer_interrupt():
    acknowledge_interrupt()
    if remaining > 0:
        write_printer_data(buffer[next])
        next += 1
        remaining -= 1
    else:
        disable_printer_interrupt()
        up(completed)
```

The initial write advances both the index and the remaining count, so the interrupt handler neither repeats the first character nor reads beyond the buffer. A semaphore remembers completion even if the interrupt arrives **before** `down(completed)`, preventing a lost wakeup. Assume the device preserves pending completion while interrupts are masked.

**(iii) DMA: the controller moves the whole buffer.**

```text
mapping = pin_and_dma_map(kernel_buffer)
completed = semaphore(initial=0)
program_DMA(mapping, length, printer)
start_DMA()
down(completed)
dma_unmap_and_unpin(mapping)

DMA_completion_interrupt():
    acknowledge_interrupt()
    up(completed)
```

DMA usually needs one completion interrupt per submitted transfer instead of CPU service per character. Its setup cost makes it more attractive for large transfers. All three methods must also report device errors; those details are omitted from this explanatory pseudocode.
