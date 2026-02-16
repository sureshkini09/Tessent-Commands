## Unwrapped scan chains

"Unwrapped" = Your scan chains have their own dedicated top-level ports (ts_si, ts_so) rather than sharing/multiplexing with functional I/O pins. This is cleaner architecturally but requires more package pins.
This is typically the preferred approach for:

ASIC designs with sufficient pin budget
High-performance designs where timing is critical
Designs where scan chain debug visibility is important
