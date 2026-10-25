# HONOR HLYL-WFQ9 | macOS Tahoe 26.7.1 | Ryzen 5 4600H / Renoir — accelerated graphics, hardware video, Broadcom and an honest list of limitations

I've been working on macOS Tahoe on an AMD Ryzen laptop, and wanted to share a detailed account of the hardware, the fixes involved and the limitations I've actually encountered.

The system boots and runs with Renoir graphics acceleration. Getting there involved more than making the desktop appear: graphics/video support, Broadcom connectivity, NVMe identification and power management all needed attention. This is still an experimental setup, with several features requiring workarounds or further testing. **The current graphics build has an open wallpaper/video-decoder hang report, so this is a progress report rather than a claim of complete stability.**

This write-up distinguishes current system observations from earlier functional tests. In particular, I updated NootedRed on October 24, so successful tests on the previous build should not be mistaken for a full validation of the new one.

## Hardware and software

| Component | Configuration |
| --- | --- |
| Laptop | HONOR HLYL-WFQ9, model code confirmed by the owner; original BIOS revision not independently verified |
| CPU | AMD Ryzen 5 4600H, Renoir / Zen 2, 6 cores and 12 threads |
| Integrated GPU | AMD Radeon Renoir, PCI ID `1002:1636` |
| Graphics memory reported now | 2 GB shared memory; this is the driver-reported allocation, not dedicated VRAM |
| RAM | 16 GB DDR4, 2 × 8 GB Hynix; macOS reports 2667 MHz |
| Internal display | 1920 × 1080 |
| SSD | WD Blue SN580 1 TB, firmware `281040WD` |
| Audio codec | Realtek ALC256, using VoodooHDA |
| Wi-Fi / Bluetooth | Replacement Broadcom BCM4352 card / Broadcom 20702 USB Bluetooth |
| Webcam | Azurewave OV9734 |
| Battery | DYNAPACK HB6081V1ECW-22T |
| macOS | Tahoe 26.7.1, build `25G241` |
| Bootloader | OpenCore 1.0.8 RELEASE; installed OpenCore binary matches the local X64 release package |
| SMBIOS | `MacBookPro16,2` — the emulated Apple identity |

I have inspected the current internal EFI, rather than substituting an older backup. The documentation repository records the enabled ACPI tables, drivers, kext versions, AMD patch ranges and relevant configuration values.

## Graphics: accelerated Renoir desktop

The current setup uses the **steve12311 NootedRed fork, v0.9.0-pre-release.9**. The loaded kext reports the internal version `0.9.0`; I matched its binary UUID to the installation record to distinguish it from the older build with the same internal version.

The relevant graphics arguments currently in use are:

```text
-NRedDisplayFix -NRedVideoFix
```

The previous configuration used `-NRedComputeScratch -NRedVCN`. Those are historical details, not the arguments used by this current build.

macOS currently reports Metal 3 support. On October 20, a Metal compute test processed 65,536 elements with correct results. A separate 30-second combined CPU/GPU test also completed without GPU errors. These are useful basic checks, but they are short tests on an earlier NootedRed build, not evidence that every graphics workload works.

There is a history of GPU resets and kernel panics on earlier configurations. One October 21 panic led to investigation of the NootedRed/shared-cache/Bluetooth path, but the exact cause was not proven.

**The current build also has a documented problem:** on October 24, scrolling through System Settings → Wallpaper preceded a graphical freeze. Seven GPU-reset reports identified `VCN0Dec` and `VTDecoderXPCServ`, followed by a WindowServer watchdog timeout. This was one observed incident; I have not established reliable reproduction, the exact preview/codec or the underlying defect. The [open upstream report](https://github.com/steve12311/NootedRed/issues/5) contains the evidence and configuration caveats. It is not proof that NootedRed alone caused the failure.

The maintainer asked about `allowMetalTransferSession`. My latest response records that no explicit setting was present in the system or user preference locations checked, and that I had not applied the proposed override. No fix is claimed.

## Hardware video: what was actually tested

Earlier VideoToolbox tests confirmed hardware H.264 and HEVC decoding at 720p and 1080p. In the October 17 tests, hardware decoding was explicitly required, `usingHardware` was confirmed, and the decoded frames matched software decoding across 30 frames per test.

An October 20 smoke test also exercised hardware H.264/HEVC encoding and decoding successfully. That encoding test involved only five 720p frames per codec, so it should be read as a pipeline check rather than an editing/export benchmark.

I have not established support for every profile, 4K playback, HDR, HEVC Main10, VP9, AV1, protected streaming, screen sharing or prolonged video workloads on the current build. I would not infer application compatibility from the VideoToolbox results alone.

## Wi-Fi, Bluetooth and AirDrop

I replaced the original Realtek wireless hardware with a BCM4352-based card. Wi-Fi has worked after the Broadcom setup, including a 5 GHz 802.11ac connection and successful basic network tests.

Bluetooth is more complicated. The controller and an HONOR AD20 mouse have worked, but reliable operation has required local recovery/reconnection helpers outside the EFI.

**AirDrop has transferred files in both directions**, with a specific workaround: the mouse reconnection service had to be paused. Simply switching the mouse off was not sufficient, because the system continued attempting reconnection. After the transfers, the mouse could be restored without pairing again.

For that reason, my status for AirDrop is **working with a workaround**. Simultaneous Bluetooth mouse use, AirDrop and Instant Hotspot is still unresolved.

iPhone hotspot connections have succeeded, including manual selection from the Wi-Fi menu. Automatic Instant Hotspot has also caused Bluetooth resets in earlier tests, so I consider it intermittent rather than fully working.

An EFI folder alone does not reproduce these OS-level helpers or the full wireless setup.

## SSD: fixing the “external drive” classification

The WD SN580 initially appeared as external storage. The issue was an injected `built-in` property on the wrong PCI root.

The actual NVMe path on this machine is:

```text
PciRoot(0x1)/Pci(0x2,0x4)/Pci(0x0,0x0)
```

Moving the `built-in` data property (`01`) to that path fixed the classification after reboot. macOS subsequently identified the SSD and system volume as internal, while TRIM and the reported SMART status remained available. A temporary 256 MiB write/read checksum test passed.

That path is specific to this machine. It is included to explain the diagnosis, not as a universal SN580 fix. Earlier logs also contained `AppleNVMe Assert failed` messages; their cause was not established, and correcting the classification does not prove those messages harmless.

## CPU power management and battery

The current loaded drivers include `AMDRyzenCPUPowerManagement` and `SMCAMDProcessor`, both reporting `3.34.0`.

After their installation, the CPU driver identified the 6-core/12-thread Zen 2 processor and exposed active P-state management. Some `SMCAMDProcessor` sensor readings and expected VirtualSMC keys were missing or inconsistent, so I do not treat every displayed temperature or power value as reliable.

The battery is also worn: earlier readings placed full-charge capacity around 70% of design capacity, with over 520 cycles. Remaining-time estimates were inconsistent with other readings. I have not measured a complete discharge cycle, and I cannot separate hardware wear from driver/reporting effects well enough to give an honest battery-life figure.

## Feature status

| Feature | Status and qualification |
| --- | --- |
| Boot from internal storage | Previously confirmed; repeated cold-boot and recovery testing still needed |
| Internal FHD display / GPU driver | Present now; accelerated workloads passed earlier short tests |
| H.264 / HEVC hardware video | Earlier targeted tests passed; current build needs repeat testing |
| NVMe / internal classification / TRIM | Classification reboot-verified; TRIM currently reported enabled |
| Wi-Fi | Worked in earlier connection tests |
| Bluetooth mouse | Worked with recovery/reconnection helpers |
| AirDrop | Bidirectional transfers succeeded with mouse reconnection paused |
| Instant Hotspot | Intermittent; manual connections succeeded |
| Battery reporting | Available, with wear and estimate inconsistencies |
| CPU power management | Driver loaded; P-state operation observed previously |
| Audio / microphone / headphone jack | ALC256 and VoodooHDA identified; full physical checks pending |
| Webcam | OV9734 and AzurewaveUVCFix loaded; current capture stability unverified |
| Keyboard / trackpad | PS/2 and I2C drivers loaded; complete key/gesture testing pending |
| Sleep / wake | Earlier cycles recorded; full peripheral recovery and overnight drain unverified |
| USB ports | Mapping present historically; all physical ports/speeds/orientations not fully tested |
| HDMI / external display | Not verified |
| Fingerprint reader | Device enumerated; authentication not verified |
| iMessage / FaceTime / Handoff | Not verified in this write-up |
| DRM / protected streaming | Not verified |
| Long-term stability | Not established; current build has an open wallpaper/video-decoder hang report |

## Security and reproducibility caveats

The Broadcom setup uses system patches and reduced protections. The current read-only check reports custom SIP settings with kext signing and filesystem protections disabled, and Authenticated Root disabled. The current EFI has `SecureBootModel=Disabled`. The upstream incident report also records OCLP-Plus 3.2.2 Modern Wireless root-patch metadata affecting `wifip2pd`, `IO80211.framework` and `WiFiPeerToPeer.framework`. This is not a clean, unpatched macOS baseline, and copying boot files alone is not the complete installation process.

I also want to avoid presenting a collection of kexts as a portable installer. Firmware settings, AMD kernel patches, device paths, USB mapping and OS-level patches all need to match the target machine.

## What I still want to improve

My priorities are repeatable Bluetooth/AirDrop coexistence, reliable hotspot activation, prolonged graphics/video testing on the latest build, and proper checks of audio, webcam, sleep, external displays and battery life.

If you have a similar Renoir laptop, I'd be interested in comparable results that include the exact macOS and driver builds, the workload tested and any workarounds required. Those details are much more useful than a general “everything works.”

## Credits

Thanks to the OpenCore/Acidanthera contributors, AMD-OSX, ChefKiss/NootedRed, steve12311's NootedRed fork, the Voodoo driver contributors, the Broadcom patching projects and the Dortania documentation contributors.

## Detailed documentation

[Hardware, configuration inventory, test history and outstanding issues](https://github.com/Skensy/hlyl-wfq9-hackintosh-tahoe). This is a documentation-only repository, with no EFI download.

The original BIOS revision and a complete physical-device test pass remain unverified. I have left those gaps visible rather than filling them with assumptions. This write-up was prepared with AI assistance from the machine's configuration and test records, then checked against the available evidence.
