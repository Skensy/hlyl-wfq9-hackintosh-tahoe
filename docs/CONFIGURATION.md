# Current EFI configuration inventory

Read from the internal EFI on October 25, 2026. Model code HLYL-WFQ9 confirmed by the owner. This is a configuration record, not a boot-tested installation guide.

## Bootloader

The installed `OpenCore.efi` is byte-identical to the local X64 OpenCore 1.0.8 RELEASE package. The original firmware/BIOS revision was not independently recovered.

## Enabled ACPI tables

- `SSDT-ALS0.aml`: SSDT-ALS0.aml
- `SSDT-EC.aml`: SSDT-EC.aml
- `SSDT-PLUG-ALT.aml`: SSDT-PLUG-ALT.aml
- `SSDT-PNLF.aml`: SSDT-PNLF.aml
- `SSDT-USB-Reset.aml`: SSDT-USB-Reset.aml
- `SSDT-USBX.aml`: SSDT-USBX.aml
- `SSDT-XOSI.aml`: SSDT-XOSI.aml
- `SSDT-GPIO.aml`: SSDT-GPIO.aml - Huawei MateBook D AMD GPIO Interrupts

## UEFI drivers

| Path | Enabled | Load early |
| --- | --- | --- |
| `apfs_aligned.efi` | True | False |
| `HfsPlus.efi` | True | False |
| `OpenRuntime.efi` | True | False |
| `ResetNvramEntry.efi` | True | False |
| `OpenCanopy.efi` | True | False |

## Kernel additions

This records configured entries, including disabled ones. A configured entry is not proof it loaded or that its hardware works. Empty kernel bounds mean the configuration does not constrain that end of the range.

| Bundle | Version | Enabled | Minimum kernel | Maximum kernel |
| --- | --- | --- | --- | --- |
| `AppleMCEReporterDisabler.kext` | 1.2 | True |  |  |
| `Lilu.kext` | 1.7.3 | True |  |  |
| `AzurewaveUVCFix.kext` | 1.0.2 | True | 25.6.0 | 25.6.99 |
| `NootedRed.kext` | 0.9.0 | True |  |  |
| `NVMeFix.kext` | 1.1.4 | True |  |  |
| `RealtekBluetoothFirmware.kext` | 1.0.2 | False |  |  |
| `RestrictEvents.kext` | 1.1.7 | True |  |  |
| `USBToolBox.kext` | 1.2.0 | True |  |  |
| `UTBMap.kext` | 1.1 | True |  |  |
| `VirtualSMC.kext` | 1.3.9 | True |  |  |
| `AMDRyzenCPUPowerManagement.kext` | 3.34.0 | True | 25.0.0 | 25.99.99 |
| `SMCAMDProcessor.kext` | 3.34.0 | True | 25.0.0 | 25.99.99 |
| `VoodooPS2Controller.kext` | 2.3.8 | True |  |  |
| `VoodooI2C.kext/Contents/PlugIns/VoodooGPIO.kext` | 1.1 | True |  |  |
| `VoodooI2C.kext/Contents/PlugIns/VoodooI2CServices.kext` | 1 | True |  |  |
| `VoodooI2C.kext/Contents/PlugIns/VoodooInput.kext` | 1.1.7 | True |  |  |
| `VoodooPS2Controller.kext/Contents/PlugIns/VoodooInput.kext` | 1.1.7 | False |  |  |
| `VoodooPS2Controller.kext/Contents/PlugIns/VoodooPS2Keyboard.kext` | 2.3.8 | True |  |  |
| `VoodooPS2Controller.kext/Contents/PlugIns/VoodooPS2Mouse.kext` | 2.3.8 | False |  |  |
| `VoodooPS2Controller.kext/Contents/PlugIns/VoodooPS2Trackpad.kext` | 2.3.8 | False |  |  |
| `BrcmFirmwareData.kext` | 2.7.2 | True | 25.0.0 | 25.99.99 |
| `BrcmPatchRAM3.kext` | 2.7.2 | True | 25.0.0 | 25.99.99 |
| `BlueToolFixup.kext` | 2.7.3 | True | 21.0.0 |  |
| `BrightnessKeys.kext` | 1.0.4 | True |  |  |
| `ForgedInvariant.kext` | 1.5.0 | True |  |  |
| `SMCBatteryManager.kext` | 1.3.9 | True |  |  |
| `SMCLightSensor.kext` | 1.3.9 | True |  |  |
| `VoodooI2C.kext` | 2.9.1 | True |  |  |
| `VoodooI2CHID.kext` | 1 | True |  |  |
| `AMFIPass.kext` | 1.4.1 | True | 25.0.0 | 25.99.99 |
| `AirportBrcmFixup.kext` | 2.2.1 | True | 25.0.0 | 25.99.99 |
| `IOSkywalkFamily.kext` | 1.0 | True | 25.0.0 | 25.99.99 |
| `IO80211FamilyLegacy.kext` | 1200.12.2b1 | True | 25.0.0 | 25.99.99 |
| `IO80211FamilyLegacy.kext/Contents/PlugIns/AirPortBrcmNIC.kext` | 1400.1.1 | True | 25.0.0 | 25.99.99 |
| `AirportBrcmFixup.kext/Contents/PlugIns/AirPortBrcmNIC_Injector.kext` | 1.0 | True | 25.0.0 | 25.99.99 |
| `AirPort_RTW88.kext` | 2.0.1 | False | 25.0.0 | 25.99.99 |
| `VoodooHDA_BL.kext` | 3.7.0 | True | 25.0.0 | 25.99.99 |

NootedRed internally reports `0.9.0`; the running binary UUID matches the recorded steve12311 v0.9.0-pre-release.9 installation. This distinction matters because earlier builds reported the same internal version.

## Kernel patches

The following comments/ranges are copied as configuration metadata. They do not prove patch provenance, correctness on every OS, or support outside the configured range.

| Comment | Enabled | Kernel bounds |
| --- | --- | --- |
| algrey  /  Force cpuid_cores_per_package to constant (user-specified)  /  10.13-10.14 | True | 17.0.0 → 18.99.99 |
| algrey  /  Force cpuid_cores_per_package to constant (user-specified)  /  10.15-11.0 | True | 19.0.0 → 20.99.99 |
| algrey  /  Force cpuid_cores_per_package to constant (user-specified)  /  12.0-13.2 | True | 21.0.0 → 22.3.99 |
| algrey  /  Force cpuid_cores_per_package to constant (user-specified)  /  13.3+ | True | 22.4.0 → 25.99.99 |
| algrey  /  _commpage_populate  /  Remove rdmsr  /  10.13+ | True | 17.0.0 → 25.99.99 |
| algrey  /  _cpuid_set_cache_info  /  Set CPUID proper instead of 4  /  10.13+ | True | 17.0.0 → 25.99.99 |
| algrey  /  _cpuid_set_generic_info  /  Remove wrmsr(0x8B)  /  10.13+ | True | 17.0.0 → 25.99.99 |
| algrey  /  _cpuid_set_generic_info  /  Replace rdmsr(0x8B) with constant 186  /  10.13+ | True | 17.0.0 → 25.99.99 |
| algrey  /  _cpuid_set_generic_info  /  Set flag=1  /  10.13+ | True | 17.0.0 → 25.99.99 |
| algrey  /  _cpuid_set_generic_info  /  Disable check to allow leaf7  /  10.13+ | True | 17.0.0 → 23.99.99 |
| algrey  /  _cpuid_set_generic_info  /  Disable check to allow leaf7  /  15.0+ | True | 24.0.0 → 25.99.99 |
| algrey  /  _cpuid_set_info  /  GenuineIntel to AuthenticAMD  /  10.13-11.0 | True | 17.0.0 → 20.99.99 |
| Goldfish64, algrey  /  Bypass GenuineIntel check panic  /  12.0+ | True | 21.0.0 → 25.99.99 |
| algrey  /  _cpuid_set_cpufamily  /  Force CPUFAMILY_INTEL_PENRYN  /  10.13-11.2 | True | 17.0.0 → 20.3.0 |
| algrey  /  _cpuid_set_cpufamily  /  Force CPUFAMILY_INTEL_PENRYN  /  11.3+ | True | 20.4.0 → 25.99.99 |
| algrey  /  _i386_init  /  Remove 3 rdmsr calls  /  10.13+ | True | 17.0.0 → 25.99.99 |
| algrey, XLNC  /  Remove version check and panic  /  10.13+ | True | 17.0.0 → 25.99.99 |
| CaseySJ  /  probeBusGated  /  Disable 10 bit tags  /  12.0-15.x | True | 21.0.0 → 24.99.99 |
| CaseySJ  /  probeBusGated  /  Disable 10 bit tags  /  26.0+ | True | 25.0.0 → 25.99.99 |
| CaseySJ  /  IOPCIIsHotplugPort  /  Fix PCI bus enumeration on AM5  /  13.0+ | False | 22.0.0 → 25.99.99 |
| Visual  /  thread_quantum_expire, thread_unblock, thread_invoke  /  Remove non-monotonic time panic  /  12.0+ | True | 21.0.0 → 25.99.99 |
| Visual  /  thread_invoke, thread_dispatch  /  Remove non-monotonic time panic  /  12.0-26.3 | True | 21.0.0 → 25.3.99 |
| laobamac  /  thread_invoke, thread_dispatch  /  Remove non-monotonic time panic  /  26.4+ | True | 25.4.0 → 25.99.99 |
| algrey  /  _mtrr_update_action  /  fix PAT  /  10.13+ | False | 17.0.0 → 23.99.99 |
| Shaneee  /  _mtrr_update_action  /  Fix PAT  /  10.13+ | True | 17.0.0 → 23.99.99 |
| Algrey / Zormeister  /  _mtrr_update_action  /  Fix PAT  /  15.0+ | False | 24.0.0 → 25.99.99 |
| Shaneee / Zormeister  /  _mtrr_update_action  /  Fix PAT  /  15.0+ | True | 24.0.0 → 25.99.99 |

### Core-count replacement entries

- 17.0.0 → 18.99.99: replacement bytes `b80600000000`.
- 19.0.0 → 20.99.99: replacement bytes `ba0600000000`.
- 21.0.0 → 22.3.99: replacement bytes `ba0600000090`.
- 22.4.0 → 25.99.99: replacement bytes `ba06000000`.

## Boot arguments and selected security values

```text
debug=0x100 keepsyms=1 -amfipassbeta -NRedDisplayFix -NRedVideoFix revcpu=0
```

| Setting | Value |
| --- | --- |
| PlatformInfo/Automatic | True |
| PlatformInfo/Generic/SystemProductName | MacBookPro16,2 |
| PlatformInfo/Generic/ProcessorType | 1537 |
| PlatformInfo/Generic/SpoofVendor | True |
| Misc/Security/SecureBootModel | Disabled |
| Misc/Security/DmgLoading | Signed |
| Misc/Security/Vault | Optional |
| Misc/Security/ScanPolicy | 0 |
| Misc/Security/ExposeSensitiveData | 6 |
| NVRAM csr-active-config, raw data bytes | `03080000` |

Current `csrutil` reads report custom SIP with kext signing and filesystem protections disabled; Authenticated Root is disabled. These protections were not changed for this publication.

## Device properties

| PCI path | Property names |
| --- | --- |
| `PciRoot(0x1)/Pci(0x2,0x4)/Pci(0x0,0x0)` | `built-in` |
| `PciRoot(0x0)/Pci(0x8,0x1)/Pci(0x0,0x6)` | `layout-id` |

The NVMe `built-in` property belongs to `PciRoot(0x1)/Pci(0x2,0x4)/Pci(0x0,0x0)` on this machine. See the build write-up for its reboot-verified effect.

## Selected boot and kernel settings

| Setting | Value |
| --- | --- |
| Booter/Quirks/AllowRelocationBlock | False |
| Booter/Quirks/AvoidRuntimeDefrag | True |
| Booter/Quirks/ClearTaskSwitchBit | False |
| Booter/Quirks/DevirtualiseMmio | False |
| Booter/Quirks/DisableSingleUser | False |
| Booter/Quirks/DisableVariableWrite | False |
| Booter/Quirks/DiscardHibernateMap | False |
| Booter/Quirks/EnableSafeModeSlide | True |
| Booter/Quirks/EnableWriteUnprotector | False |
| Booter/Quirks/FixupAppleEfiImages | True |
| Booter/Quirks/ForceBooterSignature | False |
| Booter/Quirks/ForceExitBootServices | False |
| Booter/Quirks/ProtectMemoryRegions | False |
| Booter/Quirks/ProtectSecureBoot | False |
| Booter/Quirks/ProtectUefiServices | False |
| Booter/Quirks/ProvideCustomSlide | True |
| Booter/Quirks/ProvideMaxSlide | 0 |
| Booter/Quirks/RebuildAppleMemoryMap | True |
| Booter/Quirks/ResizeAppleGpuBars | -1 |
| Booter/Quirks/SetupVirtualMap | True |
| Booter/Quirks/SignalAppleOS | False |
| Booter/Quirks/SyncRuntimePermissions | True |
| Kernel/Quirks/AppleCpuPmCfgLock | False |
| Kernel/Quirks/AppleXcpmCfgLock | False |
| Kernel/Quirks/AppleXcpmExtraMsrs | False |
| Kernel/Quirks/AppleXcpmForceBoost | False |
| Kernel/Quirks/CustomPciSerialDevice | False |
| Kernel/Quirks/CustomSMBIOSGuid | True |
| Kernel/Quirks/DisableIoMapper | True |
| Kernel/Quirks/DisableIoMapperMapping | False |
| Kernel/Quirks/DisableLinkeditJettison | True |
| Kernel/Quirks/DisableRtcChecksum | False |
| Kernel/Quirks/ExtendBTFeatureFlags | False |
| Kernel/Quirks/ExternalDiskIcons | False |
| Kernel/Quirks/ForceAquantiaEthernet | False |
| Kernel/Quirks/ForceSecureBootScheme | False |
| Kernel/Quirks/IncreasePciBarSize | False |
| Kernel/Quirks/LapicKernelPanic | False |
| Kernel/Quirks/LegacyCommpage | False |
| Kernel/Quirks/PanicNoKextDump | False |
| Kernel/Quirks/PowerTimeoutKernelPanic | True |
| Kernel/Quirks/ProvideCurrentCpuInfo | True |
| Kernel/Quirks/SetApfsTrimTimeout | -1 |
| Kernel/Quirks/ThirdPartyDrives | False |
| Kernel/Quirks/XhciPortLimit | False |
| Misc/Boot/ConsoleAttributes | 0 |
| Misc/Boot/HibernateMode | None |
| Misc/Boot/HibernateSkipsPicker | False |
| Misc/Boot/HideAuxiliary | True |
| Misc/Boot/InstanceIdentifier |  |
| Misc/Boot/LauncherOption | Full |
| Misc/Boot/LauncherPath | Default |
| Misc/Boot/PickerAttributes | 17 |
| Misc/Boot/PickerAudioAssist | False |
| Misc/Boot/PickerMode | External |
| Misc/Boot/PickerVariant | Acidanthera\GoldenGate |
| Misc/Boot/PollAppleHotKeys | False |
| Misc/Boot/ShowPicker | True |
| Misc/Boot/TakeoffDelay | 0 |
| Misc/Boot/Timeout | 10 |

## Scope

No serial number, MLB, ROM or machine UUID is included here. Standard firmware GUIDs, PCI IDs and software-version identifiers describe the configuration and are distinct from the private SMBIOS identity. No raw private logs or original configuration files are distributed by this documentation repository.
