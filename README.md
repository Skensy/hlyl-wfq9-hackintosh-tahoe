# HONOR HLYL-WFQ9 — macOS Tahoe 26.7.1 build documentation

**Configuration snapshot: October 25, 2026. Experimental build; known graphics/video hang remains open.**

This repository documents a working macOS installation on a Huawei/Honor AMD laptop, including the experiments, partial successes and outstanding limitations. The owner confirmed the model code **HLYL-WFQ9**. The original BIOS revision remains unverified. The Apple model shown by macOS is an emulated identity.

## Hardware

| Item | Configuration |
| --- | --- |
| CPU | AMD Ryzen 5 4600H, 6 cores / 12 threads |
| GPU | Radeon Renoir `1002:1636`; currently reports 2 GB shared graphics memory |
| RAM | 16 GB, 2 × 8 GB Hynix DDR4; reported speed 2667 MHz |
| Screen | Internal 1920 × 1080 panel |
| Storage | WD Blue SN580 1 TB, firmware `281040WD` |
| Audio | Realtek ALC256 / VoodooHDA |
| Wireless | Broadcom BCM4352 replacement / Broadcom 20702 USB Bluetooth |
| Camera | Azurewave OV9734 |
| Battery | DYNAPACK HB6081V1ECW-22T |
| macOS | Tahoe 26.7.1 (`25G241`) |
| SMBIOS | `MacBookPro16,2` |

## Read first

See [the detailed build write-up](REDDIT-POST.md) for feature status, test dates, workarounds and limitations. The write-up separates currently observed configuration, earlier successful tests and known failures. See also the [configuration inventory](docs/CONFIGURATION.md) and [testing and limitations](docs/TESTING.md).

This repository contains **documentation only**, with no EFI, boot binaries, original SMBIOS identifiers or private diagnostic logs. Its configuration inventory was generated from the current internal EFI, which was inspected after the owner mounted it.

OS-level patches and recovery helpers are part of this installation. An EFI copy alone cannot reproduce the complete system, wireless behavior or security configuration. The inventory describes this machine; it is not a recommendation to copy its settings to another device.

## Current loaded-driver observations

The following versions were read from the running system on October 25. This is a selected runtime inventory, not the final `Kernel/Add` inventory or load order.

| Driver | Reported version |
| --- | --- |
| Lilu | 1.7.3 |
| NootedRed | 0.9.0; binary matches recorded v0.9.0-pre-release.9 installation |
| VirtualSMC / SMCBatteryManager | 1.3.9 |
| AMDRyzenCPUPowerManagement / SMCAMDProcessor | 3.34.0 |
| RestrictEvents | 1.1.7 |
| AMFIPass | 1.4.1 |
| AirportBrcmFixup | 2.2.1 |
| BrcmFirmwareStore / BrcmPatchRAM3 | 2.7.2 |
| BlueToolFixup | 2.7.3 |
| VoodooHDA | 3.7.0 |
| VoodooI2C | 2.9.1 |
| VoodooPS2Controller / VoodooPS2Keyboard | 2.3.8 |
| VoodooInput | 1.1.7 |
| AzurewaveUVCFix | 1.0.2 |

The running boot arguments currently read:

```text
debug=0x100 keepsyms=1 -amfipassbeta -NRedDisplayFix -NRedVideoFix revcpu=0
```

These are a record of this installation, not instructions to apply them to another computer. The final EFI review must check enabled patches, their kernel ranges, drivers, ACPI, device properties and security settings independently.

## Changes with verified outcomes

- **NVMe classification:** the `built-in` property needed the actual device path `PciRoot(0x1)/Pci(0x2,0x4)/Pci(0x0,0x0)`. Internal classification was confirmed after reboot on October 20. TRIM is currently reported enabled.
- **AirDrop:** bidirectional transfers succeeded when the HONOR AD20 mouse reconnection service was paused. Simultaneous use remains unresolved. OS helpers were involved.
- **Graphics/video:** earlier short Metal and VideoToolbox tests passed. NootedRed changed on October 24, so those results remain historical until repeated on the new build.
- **CPU power management:** driver loading and P-state operation were observed. Some sensor readings were inconsistent and are not reliable measurements.

## Current known failure

On October 24, scrolling the wallpaper list preceded a graphical freeze on NootedRed pre-release.9. Seven GPU-reset reports named `VCN0Dec` / `VTDecoderXPCServ`; WindowServer then reported a watchdog timeout. This is one observed incident, not a reliably reproduced root-cause diagnosis. See [the open report and maintainer discussion](https://github.com/steve12311/NootedRed/issues/5).

The maintainer requested checking `allowMetalTransferSession`. The owner's latest reply says it was not explicitly set in the preference locations checked; the suggested override had not been applied. This documentation does not claim a resolution.

## System patches and security configuration

The current read-only check shows custom SIP with kext signing and filesystem protections disabled, and Authenticated Root disabled. The EFI sets `SecureBootModel=Disabled`. These are configuration facts, not changes made while preparing this repository.

The incident report records OCLP-Plus 3.2.2 Modern Wireless metadata affecting `wifip2pd`, `IO80211.framework` and `WiFiPeerToPeer.framework`. The exact reinstall/rollback sequence has not been independently reconstructed here. A generic wireless-patching command is deliberately not presented as a tested installation procedure.

Bluetooth recovery and mouse/AirDrop switching have also depended on local OS helpers outside the EFI. Those helpers are not distributed by this documentation repository.

## Evidence and boundaries

- October 25: read-only hardware/software observations, loaded-driver versions, current boot arguments, current EFI inventory and comparison of the OpenCore binary with the local 1.0.8 RELEASE package.
- October 17–20: short VideoToolbox, Metal, CPU, memory and storage checks. These predate the latest NootedRed update.
- October 24: current-build graphical freeze documented in the upstream issue.
- Historical owner confirmations: internal-disk boot and bidirectional AirDrop using the mouse-reconnection workaround.

No EFI, BIOS, NVRAM or security setting was changed for this publication. No fresh stress test, reboot, sleep cycle or physical peripheral test was performed. The original BIOS revision, firmware settings and complete installation chronology remain unverified.

## Upstream projects

- [OpenCorePkg](https://github.com/acidanthera/OpenCorePkg)
- [AMD Vanilla kernel patches](https://github.com/AMD-OSX/AMD_Vanilla)
- [NootedRed upstream](https://github.com/ChefKissInc/NootedRed)
- [NootedRed fork used by this installation](https://github.com/steve12311/NootedRed)
- [Dortania documentation](https://dortania.github.io/)

Third-party components retain their upstream licenses. No blanket license is asserted over bundled third-party software.

Documentation was prepared with AI assistance from local records and current read-only observations. Third-party components remain the work of their respective authors.
