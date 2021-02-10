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
- [Donation](#Donate-a-coffee)

## OpenCore Version
- 0.6.6

## OS Version Supported
- Big Sure 11.2.1 with OC 0.6.6
- Big Sur 11.1 with OC 0.6.5
- Big Sur 11.1 with OC 0.6.4
- Big Sur 11.0.1 with OC 0.6.4
- Big Sur 11.0.1 with OC 0.6.3
- Catalina 10.15.7 with OC 0.6.3
- Catalina 10.15.7 with OC 0.6.2
- Catalina 10.15.7
- Catalina 10.15.6

![](https://github.com/ansonliao/Asrock-B460m-ITX-AC-OC-EFI/blob/master/images/mac_11_with_oc.jpg)

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
*2021-Feb-10*
- Upgraded OC to `0.6.6`
- Upgraded KEXTs
- Supported macOS `11.2.1`

*2021-Jan-30*
- Changed audio layout-id to 11

*2021-Jan-01*
- Upgraded OC to `0.6.5`
- Upgraded KEXTs

*2020-Dec-17*
- Supported macOS version: `11.1`

*2020-Dec-10*
- Upgraded OC to `0.6.4`
- Upgraded KEXTs

*2020-Dec-04*
- Upgraded OC to `0.6.3`
- Upgraded KEXTs
- macOS version: `11.0.1`

*2020-Dec-04*
- Upgraded OC to `0.6.3`
- Upgraded KEXTs
- macOS version: `10.15.7`

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
> IMPORTANT: Please note that the KEXTs to enable Intel WiFi/bluetooth module maybe is not update to date, please visit [OpenIntelWireless/itlwm](https://github.com/OpenIntelWireless/itlwm) to get the latest guideline, here only a simple steps before I tried to enable Intel WiFi/bluetooth module.

1. Download `itlwm.kext` from [OpenIntelWireless/itlwm](https://github.com/OpenIntelWireless/itlwm) and place the kext file to `EFI/OC/Kexts/`
2. Downlaod `IntelBluetoothInjector.kext` from [OpenIntelWireless/IntelBluetoothFirmware](https://github.com/OpenIntelWireless/IntelBluetoothFirmware) and place the kext file to `EFI/OC/Kexts/`
3. Copy `IntelMausiEthernet.kext` from `Intel WiFi/Bluetooth Module KEXTS` to directory `EFI/OC/Kexts`
4. Add `itlwm.kext`, `IntelBluetoothInjector.kext`, `IntelMausiEthernet.kext` to your `config.plist`, the section `Kernel`
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

## Donate a coffee 
![](https://github.com/ansonliao/Asrock-B460m-ITX-AC-OC-EFI/blob/master/images/donation.png?raw=true)
