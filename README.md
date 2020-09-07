# Asrock-B460m-ITX-AC-OC-EFI
The EFI of OpenCore for Asrock B460M-ITX/AC with Intel I5 10500 ES CPU and iGPU UHD 630.

## OS Version
- Catalina 10.15.6

## OpenCore Version
- 0.6.0

![](https://github.com/ansonliao/Asrock-B460m-ITX-AC-OC-EFI/blob/master/images/about_mac_catalina_10.15.6.jpg)
![](https://github.com/ansonliao/Asrock-B460m-ITX-AC-OC-EFI/blob/master/images/energy_saver.jpg)
![](https://github.com/ansonliao/Asrock-B460m-ITX-AC-OC-EFI/blob/master/images/audio_output.jpg)
![](https://github.com/ansonliao/Asrock-B460m-ITX-AC-OC-EFI/blob/master/images/audio_input.jpg)
![](https://github.com/ansonliao/Asrock-B460m-ITX-AC-OC-EFI/blob/master/images/usb_ports.jpg)

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

## What Works
- Audio device
- USB: USB 2 and USB 3
- HDMI video output port (only tested in 1920x1080@30HZ with 4K monitor, didn't test the audio of HDMI)
- DP video port, audio output of DP
- Sleep
- Wake up
- iServices: iMessage, FaceTime, Apple ID, App Store, iCloud, Sidecar (with BCM94360CS2).
- WiFI & Bluetooth
    - Intel WiFi & bluetooth also works with patches: [OpenIntelWireless/itlwm](https://github.com/OpenIntelWireless/itlwm).
    - BCM94360CS2: WiFi works, bluetooth works, iServices works.

## What Broken:
- When the OS boot, the Apple logo first is big, then changes to smaller
- In `Hackintool` -> `USB`, the `name` can't display correctly, displays `???`

## How To Enable Built Intel WiFi/Bluetooth Module
1. Copy the patch KEXTs from directory `Intel WiFi/Bluetooth Module KEXTS` (`IntelBluetoothInjector.kext`, `IntelMausiEthernet.kext`, `itlwm.kext`) to directory `EFI/OC/Kexts`.
2. Rename `config_itlwm.plist` of directory `EFI/OC` to `config.plist`.
3. Download `HeliPort` from [here](https://openintelwireless.github.io/HeliPort/) and install it.
4. Reboot with reset NVRAM.
5. Control the WiFi with `HeliPort` to on/off.
6. Enjoy it.

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
