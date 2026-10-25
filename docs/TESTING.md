# Tests, known failures and limits

Configuration reference: HONOR HLYL-WFQ9, Ryzen 5 4600H / Renoir, 16 GB RAM, WD Blue SN580 1 TB, macOS Tahoe 26.7.1 (`25G241`). Last documentation check: October 25, 2026.

## Current observations versus earlier functional tests

| Area | Evidence | Limit |
| --- | --- | --- |
| OpenCore | Installed binary matches local X64 OpenCore 1.0.8 RELEASE package | Not a new cold-boot test of a public copy |
| GPU | NootedRed loaded; internal FHD display and Metal 3 reported on October 25 | Feature reporting does not prove prolonged workload stability |
| Metal compute | October 20: 65,536 output elements correct | Earlier NootedRed build; brief synthetic workload |
| Combined CPU/GPU | October 20: 30 seconds, six CPU workers and 584 Metal runs, no GPU errors | No endurance or full thermal validation |
| CPU | October 20: six workers, ten million iterations each, coherent results | No claim of application-wide AMD compatibility |
| Memory | October 20: 256 MiB checked with two patterns, zero differences | Most of the installed 16 GB was not tested |
| Hardware decode | October 17: H.264 and HEVC, 720p/1080p, 30 frames per test, hardware use confirmed and software-output comparison passed | Earlier graphics build; no claim about DRM, HDR, all profiles or long playback |
| Hardware encode | October 20: five 720p frames per codec, H.264/HEVC hardware encoding confirmed | A smoke test, not an editor/export benchmark |
| SSD | October 20: internal classification confirmed after reboot; 256 MiB checksum passed | Read caching possible; no full NAND test or throughput benchmark |
| Storage health | TRIM enabled and SMART Verified currently reported | No detailed NVMe health-counter assessment |
| Wi-Fi | Historical 5 GHz 802.11ac connection; DNS, HTTPS and five-packet connectivity checks passed | No throughput/endurance/roaming study |
| AirDrop | Owner confirmed file transfers in both directions | Mouse reconnection service had to be paused |
| Bluetooth | Owner confirmed mouse operation after recovery/reconnection | Recovery helpers involved; simultaneous mouse/AirDrop/hotspot unresolved |
| Hotspot | Manual Wi-Fi connections succeeded historically | Automatic activation intermittent; earlier Bluetooth resets occurred |
| CPU management | P-state operation observed after installation; drivers currently loaded | Some VirtualSMC sensor values/keys inconsistent |
| Sleep | Earlier sleep/wake events recorded, including lid-close sleep | Full post-wake peripheral recovery, overnight drain and deep sleep unverified |

## Failure on the current graphics build

On October 24, the machine froze while browsing wallpaper previews with NootedRed v0.9.0-pre-release.9. The incident produced seven reports naming VCN0Dec / VTDecoderXPCServ and a WindowServer watchdog timeout. The [upstream issue](https://github.com/steve12311/NootedRed/issues/5) documents the observations, system-patch caveat and follow-up discussion.

This is a known current-build incident. Its exact cause and repeatability have not been established. Do not interpret earlier hardware-video test successes as a resolution of this failure.

## Physical features still requiring a complete test pass

Audio playback, headphones, microphone, webcam capture, keyboard shortcuts, trackpad gestures, brightness control, every physical USB port and speed, USB-C orientation, external displays/HDMI, fingerprint authentication, iMessage, FaceTime, Handoff and protected streaming remain unverified by this publication's test pass. A loaded driver or enumerated device is not treated as proof of those functions.

Battery readings previously indicated around 70% of design capacity and more than 520 cycles, with inconsistent remaining-time estimates. Those are historical software readings, not a measured battery-life result or proof that the reading precisely matches physical battery health.

## Next useful checks

Repeat targeted hardware-video and desktop workloads on the current build, document any recurrence of the wallpaper hang, then check physical peripherals, sleep recovery and a real discharge cycle. Record driver build and workload with each result. No stress test, reboot or settings change was performed while preparing this documentation.
