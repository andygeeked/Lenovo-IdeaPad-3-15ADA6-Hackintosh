# Lenovo IdeaPad 3 15ADA6 – OpenCore EFI (macOS Ventura)

OpenCore EFI for the **Lenovo IdeaPad 3 15ADA6** with **AMD Ryzen 3 3250U**,
running **macOS Ventura 13.7.8** with GPU acceleration through NootedRed.

> Experimental. Tested on one machine only. Use at your own risk.

## Tested hardware

| | |
|---|---|
| CPU | AMD Ryzen 3 3250U (2 cores / 4 threads) |
| GPU | Integrated AMD Radeon Graphics (Vega), 512 MB UMA (locked in BIOS) |
| RAM | 20 GB DDR4-2400 (4 GB soldered + 16 GB SO-DIMM) |
| SMBIOS | MacBookPro16,3 |
| macOS | Ventura 13.7.8 (last Ventura release) |

## What works / what doesn't

| Status | Feature |
|---|---|
| ✅ | Boots macOS Ventura 13.7.8 (AMD kernel patches) |
| ✅ | GPU acceleration with NootedRed |
| ✅ | Audio (AppleALC, layout-id 11) |
| ✅ | Keyboard |
| ✅ | Trackpad (VoodooPS2 + VoodooI2C) |
| ✅ | Brightness control |
| ✅ | Camera |
| ✅ | Battery and battery percentage |
| ✅ | Sleep / wake |
| ✅ | App Store sign-in (with your own SMBIOS) |
| ✅ | Internet via USB tethering (HoRNDIS) |
| ❌ | iMessage / FaceTime – not working yet (NullEthernet + SSDT-RMNE are included, `en0` still needs work) |
| ❌ | Bluetooth – no driver in this EFI (depends on the card, usually the same chip as the Wi-Fi) |
| ❌ | HDMI output – not working |
| ❌ | SD card reader – RealtekCardReader.kext is included but the reader does not work |
| ❌ | Internal Wi-Fi – no driver in this EFI (use tethering, a supported USB adapter, or swap the M.2 card for an Intel one) |
| ❌ | Fingerprint reader – not supported on macOS |
| ⚠️ | Other Ryzen APU users report no hardware video decoding and browser glitches (Chrome/Firefox) with NootedRed – use Safari |
| ⚠️ | VRAM stays at 512 MB (the BIOS hides the UMA setting) |

## Install guide

1. **Generate your own SMBIOS** with GenSMBIOS (model `MacBookPro16,3`) and replace the
   `CHANGEME` placeholders in `config.plist` → `PlatformInfo → Generic`:
   SystemSerialNumber, MLB, SystemUUID, ROM.
   Never reuse someone else's. Check the serial on checkcoverage.apple.com
   (it should be "not valid").
2. **Install macOS with the config as shipped.** NootedRed and ForgedInvariant are
   disabled on purpose (NootedRed active during the installer can make it hang or stall),
   and `NootedRed.kext` is not included.
3. **After the install finishes**, download the latest **RELEASE** build of NootedRed from
   <https://github.com/ChefKissInc/NootedRed/releases>, copy `NootedRed.kext` into
   `EFI/OC/Kexts`, set `Enabled = true` for **NootedRed.kext** and **ForgedInvariant.kext**
   (`Kernel → Add`) and reboot.
4. If you get a black screen after the verbose output, `-NRedDPDelay` is already in
   boot-args. Try a full shutdown before booting again.
5. WhateverGreen is present but **disabled** (it conflicts with NootedRed).

`SSDT-RMNE.aml` (NullEthernet) uses a placeholder MAC address (`00:11:22:33:44:55`).
iMessage / FaceTime do not work yet.

Not recommended: macOS 14 or newer. The AMD patches in this config stop at Sequoia,
and Tahoe does not work with this EFI. Ventura no longer receives security updates.

## Kexts

Lilu 1.7.0 · NootedRed (**not included** – download the latest release) · ForgedInvariant 1.5.0 ·
VirtualSMC 1.3.4 + SMCProcessorAMD + SMCRadeonSensors + SMCBatteryManager + SMCLightSensor ·
AppleALC 1.9.3 · VoodooPS2Controller 2.3.7 · VoodooI2C 2.9.1 + VoodooI2CHID ·
BrightnessKeys · ECEnabler · NVMeFix · RestrictEvents · HoRNDIS · NullEthernet ·
RealtekCardReader · USBToolBox + UTBMap · AppleMCEReporterDisabler

## Credits

Acidanthera (OpenCore, Lilu, VirtualSMC, AppleALC and more) · ChefKiss (NootedRed,
ForgedInvariant) · AMD-OSX (AMD Vanilla kernel patches) · corpnewt (GenSMBIOS) ·
RehabMan (NullEthernet) · **@AndyWindows** (original EFI for this laptop).

NootedRed is licensed for non-commercial use only. Please don't sell this EFI.

## Disclaimer

macOS is Apple's property, and Apple's license restricts running it on non-Apple
hardware. This repository is for educational purposes only.
