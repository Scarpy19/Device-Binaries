# garnet binary patches (garnet-windows branch)

Patched copies of two vendor EFI drivers so Windows ARM64 can use the USB-C port in host mode (Redmi Note 13 Pro 5G, SM7435).
These are Qualcomm-origin binaries modified by byte patching; check redistribution terms before reuse. Patch is not a source-level change.
Offsets are hex file offsets; `orig -> new` bytes in hex.

## UsbConfigDxe.efi
Original SHA-256 `4504cda7c50b6bc374cd7285975643aff2b69896ee57f1bb1767344a84ef8d9f`
Patched  SHA-256 `9382f7f2292d1bafaade2c4fdac0abef8bfbe44a250be064789919f9e4331222`
Force USB host (DFP) mode. Evidence: forced host PROVEN. The 0x1744 nop (UsbConfigCoresDeinit call before ExitBootServices) had no observed effect and is carried.

| offset | original | patched |
|---|---|---|
| 0x1744 | a90e0094 | 1f2003d5 |
| 0xa8a0 | e9031f2a09 | 290080521f |
| 0xa8b4 | 09 | 1f |
| 0xa93c | 09 | 1f |
| 0xad34 | 29 | 49 |
| 0xaec8 | 29 | 49 |
| 0xaf38 | 081140b9 | 48008052 |
| 0xb0dc | 01000014e80b40f98801f8b6 | 4a008052e80b40f9ea2b00b9 |
| 0xb174 | 29 | 49 |
| 0xb704 | 48040034 | 16000014 |
| 0xb714 | c0000054 | 12000014 |

## XhciPciEmulationDxe.efi
Original SHA-256 `e3d1b9bb765514761b37c35e4dc23f3a35ee64b15b9a5e4b811059f3cce5af1a`
Patched  SHA-256 `e04bd696407d9deb0fd24168dccef6740bfdd6af5b694a701bada6d8f56c6360`
PciIo->Map made identity via a code cave (hook at 0x3f30 jumps to cave at 0x8200). Evidence: PROVEN before other changes, then carried.

| offset | original | patched |
|---|---|---|
| 0x3f30 | c0035fd6 | b4100014 |
| 0x8200 | 00000000000000000000000000000000000000000000000000000000000000000000000000000000 | 1000a2d21f0010ebe30000541110a2d21f0011eb82000054000010cb1178bed20000118bc0035fd6 |

