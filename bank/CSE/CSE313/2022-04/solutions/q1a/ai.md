---
author: ai
via: chat
status: unverified
summary: Firmware/BIOS in ROM runs POST, finds a boot device, loads its boot sector, which loads the kernel; the kernel initialises and starts the first process.
sources: [Introduction slides 7-10, 'Tanenbaum, MOS 4e, sec. 1.3.2']
imported_from: tfsolve-questions/data/solutions/2019-20.json
changes:
  - "2026-10-31: Made the general boot sequence accurate for BIOS and UEFI while preserving a compact BIOS answer."
---
Booting must turn a reset machine into one that can load and run ordinary processes. The following stages explain that handoff:

1. **Reset into firmware.** The CPU begins at an architecture-defined reset entry and runs firmware such as BIOS or UEFI.
2. **Initialize enough hardware to load software.** Firmware checks/initializes memory and essential devices and selects a boot entry according to its configured policy.
3. **Load a boot program.** In a traditional BIOS boot, firmware loads boot-sector code, often the disk's MBR, which locates the next loader stage. UEFI normally loads an EFI executable from the EFI System Partition instead; it does not require the same MBR-code path.
4. **Load the OS kernel.** The bootloader loads the selected kernel and any required initial filesystem image into RAM, supplies boot information, and transfers control.
5. **Initialize the operating system.** The kernel establishes memory management, interrupts, scheduling and drivers, and mounts or prepares the root filesystem.
6. **Start user space.** The kernel launches the first user process, which starts services and the login or graphical environment.

For a BIOS-only exam answer, the compact chain is **reset, BIOS/POST, boot-device selection, boot sector/loader, kernel initialization, first user process**. Keep firmware, bootloader and kernel distinct: each loads or initializes the next stage.
