---
author: ai
via: chat
status: unverified
imported_from: cse313_mem_fs_solved.tex
changes:
  - "2026-10-31: Added the requested derivation, defined effective-rate fraction, and checked units."
---
Writing a segment of size $D$ requires one positioning delay $T_{position}$ followed by the transfer itself. If the peak transfer rate is $R_{peak}$, transferring the bytes takes $D/R_{peak}$ seconds, so

$$T_{total}=T_{position}+D/R_{peak}.$$

The effective rate is $D/T_{total}$. We want it to equal a fraction $F$ of the peak rate:

$$F=\frac{D}{R_{peak}T_{position}+D}.$$

Multiplying by the denominator and gathering the terms containing $D$ gives

$$D(1-F)=F R_{peak}T_{position},$$

$$\boxed{D=\frac{F}{1-F}R_{peak}T_{position}}.$$

Here $F=0.9$, $R_{peak}=100$ MB/s and $T_{position}=5$ ms $=0.005$ s. Substituting,

$$D=\frac{0.9}{1-0.9}\times100\times0.005=\boxed{4.5\text{ MB}}.$$

The units are MB/s times seconds, hence MB. Checking: the segment's transfer time is $0.045$ s; adding $0.005$ s positioning makes its useful-transfer fraction exactly $0.9$. Larger segments amortize positioning better; the model ignores cleaning, parity and other overhead, and requires $0<F<1$.
