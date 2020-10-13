# Asrock-B460m-ITX-AC-OC-EFI
The EFI of OpenCore for Asrock B460M-ITX/AC with Intel I5 10500 ES CPU and iGPU UHD 630.

- [opencore-version](#opencore-version)
- [OS Version Supported](#os-version-supported)
- [Hardware Specification](#Hardware-Specification)
- [Changelog](#Changelog)
- [What Works](#What-Works)
- [What Broken](#What-Broken)
- [How To Enable Built Intel WiFi/Bluetooth Module](#how-to-enable-built-intel-wifibluetooth-module)
- [iGPU Patching](#iGPU-Patching)
- [Notice](#Notice)

## OpenCore Version
- 0.6.2

## OS Version Supported
- [* Catalina 10.15.7 with OC 0.6.2](https://github.com/ansonliao/Asrock-B460m-ITX-AC-OC-EFI/commit/c74984ff0530880a3ab232c4b9e29e5af3e3c37e)
- [Catalina 10.15.7 with OC 0.6.1](https://github.com/ansonliao/Asrock-B460m-ITX-AC-OC-EFI/tree/ff2944fb90fb687fecf00f22cd0f17712c79c99c)
- [Catalina 10.15.6](https://github.com/ansonliao/Asrock-B460m-ITX-AC-OC-EFI/tree/f955c264c2b699fdcd3c1e202609a5cc3afa0047)
>`*` indicated my running version, only FYI

![](https://github.com/ansonliao/Asrock-B460m-ITX-AC-OC-EFI/blob/master/images/about_mac_catalina_10.15.6.jpg)

## Hardware Specification
| Item | Brand | Comment |
| --- | --- | --- |
| CPU | Intel I5 10500 ES (QSRK) | |
| Motherboard | [Asrock B460m-ITX/AC](https://www.asrock.com/mb/Intel/B460M-ITXac/) |
| BIOS Version | 1.4.0 | |
| Memory | Cuso DDR4 2666 16G x 1 | |
| SSD | Intel 760P 512 GB | |
| iGPU | Intel UHD 630 | |
| dGPU | None | |
| WiFi/Bluetooth Module | 1. Onboard WiFi, Bluetooth Module; 2. BCM94360CS2 | |
| Case | [Inwin Chopin Mini-ITX case](https://www.amazon.com/InWin-Chopin-Mini-ITX-stickers-Aluminum/dp/B01N091225/ref=sr_1_1?crid=SLF1ACIIUQSA&dchild=1&keywords=inwin+chopin&qid=1599453831&sprefix=inwin+c%2Caps%2C345&sr=8-1) | |
| PSU | 150W 80 plus bronze adapter built in case | 
| Monitor | ViewSonic VX2831-4K-HD 28 inch | DP port connection in use |

## Changelog
*2020-Oct-09*
- Upgraded OC to `0.6.2`
- Upgraded KEXTs

*2020-Sep-29*
- Supported [Catalina 10.15.7](https://support.apple.com/kb/DL2051)

*2020-Sep-8*
- Upgraded OpenCore version to `0.6.1`
- Upgraded `AppleALC.kext` version to `1.5.2`, `Lilu.kext` version to `1.4.7`, `WhateverGreen.kext` version to `1.4.2`
- Support audio device `Realtek ALC887` natively by the latest version `AppleALC.kext` and `Lilu.kext`, set `layout-id = 0C000000` or add boot-args `alcid=12` to enable audio device `Realtek ALC887`
- Removed fake PCI ID kexts that sovled audio device before: `FakePCIID_Intel_HDMI_Audio.kext`, `FakePCIID.kext`
- Removed unnecessary files

*2020-Sep-07*
- First commit

## What Works
- Audio device: front panel, back panel
- USB: USB 2 and USB 3, total 6 USB physical ports
- HDMI video output port (only tested in 1920x1080@30HZ with 4K monitor, didn't test the audio of HDMI)
- DP video port, audio output of DP
- Sleep
- Wake up
- iServices: iMessage, FaceTime, Apple ID, App Store, iCloud, Sidecar (with BCM94360CS2)
- WiFI & Bluetooth
    - Intel WiFi & bluetooth also works with patches: [OpenIntelWireless/itlwm](https://github.com/OpenIntelWireless/itlwm)
    - BCM94360CS2: WiFi works, bluetooth works, iServices works

## What Broken
- When the OS boot, the Apple logo first is big, then changes to smaller
- In `Hackintool` -> `USB`, the `name` can't display correctly, displays `???`

## How To Enable Built Intel WiFi/Bluetooth Module
1. Download `itlwm.kext` from [OpenIntelWireless/itlwm](https://github.com/OpenIntelWireless/itlwm) and place the kext file to `EFI/OC/Kexts/`
2. Downlaod `IntelBluetoothInjector.kext` from [OpenIntelWireless/IntelBluetoothFirmware](https://github.com/OpenIntelWireless/IntelBluetoothFirmware) and place the kext file to `EFI/OC/Kexts/`
3. Copy `IntelMausiEthernet.kext` from `Intel WiFi/Bluetooth Module KEXTS` to directory `EFI/OC/Kexts`
4. Rename `config_itlwm.plist` of directory `EFI/OC` to `config.plist`
5. Check `config.plist` (just renamed in step 4) file's structure whether is match to your corresponding `OpenCore` version, if doesn't match, please update the structure of `config.plist`, more details please refer to [the official guideline of OpenCore](https://dortania.github.io/OpenCore-Post-Install/universal/update.html#updating-opencore).
5. Download `HeliPort` from [here](https://openintelwireless.github.io/HeliPort/) and install it
6. Reboot with reset NVRAM
7. Control the WiFi with `HeliPort` to on/off
8. Enjoy it

## iGPU Patching

**Purpose:** I meet black screen issue of DP port when the long time sleep (for example, more than 30 mins), when wake up, the fan of CPU start to work, but the monitor no signal, so for fixing this issue, I patched the video ports of iGPU, after patched, the black issue was fixed.

Follow the official instruction: https://dortania.github.io/OpenCore-Install-Guide/extras/gpu-patches.html#igpu-busid-patching

Then add the patching information to `DeviceProperties -> PciRoot(0x0)/Pci(0x2,0x0)`: 
```
- framebuffer-con0-enable = 01000000							// HDMI port
- framebuffer-con1-enable = 00000000							// disabled it as this motherboard only two video output port
- framebuffer-con2-enable = 01000000							// DP port
- framebuffer-con0-alldata = 01050900 00080000 C7030000 		// data type: DATA
- framebuffer-con1-alldata = 02040A00 00040000 C7030000 		// data type: DATA
- framebuffer-con2-alldata = 03060800 00040000 C7030000 		// data type: DATA
- framebuffer-patch-enable = 01000000 							// data type: DATA
```
![](https://github.com/ansonliao/Asrock-B460m-ITX-AC-OC-EFI/blob/master/images/igpu_pacthing.png)
![](https://github.com/ansonliao/Asrock-B460m-ITX-AC-OC-EFI/blob/master/images/hackintool_video_connectors.jpg)

## Notice
1. `device-id` is a must for DeviceProperties->PciRoot(0x0)/Pci(0x2,0x0), or you may suffer crashes on firefox, Photos etc.
2. Fill your SMBIOS information (can generate from [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)) in `PlatForm->Generic`, else will can't boot.
