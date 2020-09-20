<img src="Docs/img/XiaoMi_Hackintosh_with_text_Small.png" width="934" height="48"/>

[![Build Status](https://travis-ci.com/daliansky/XiaoMi-Pro-Hackintosh.svg?branch=master)](https://travis-ci.com/daliansky/XiaoMi-Pro-Hackintosh) [![release](https://img.shields.io/badge/download-release-blue.svg)](https://github.com/daliansky/XiaoMi-Pro-Hackintosh/releases) [![wiki](https://img.shields.io/badge/support-wiki-green.svg)](https://github.com/daliansky/XiaoMi-Pro-Hackintosh/wiki) [![Chat](https://img.shields.io/badge/chat-tonymacx86-red.svg)](https://www.tonymacx86.com/threads/guide-xiaomi-mi-notebook-pro-high-sierra-10-13-6.242724)
-----

macOS Big Sur & Catalina & Mojave & High Sierra on XiaoMi NoteBook Pro 2017 & 2018

**English** | [中文](README_CN.md)

## Contents

- [Configuration](#configuration)
- [Current Status](#current-status)
  - [Clover](#clover)
  - [OpenCore](#opencore)
- [Installation](#installation)
  - [First-time installation](#first-time-installation)
  - [Build](#build)
  - [Upgrade](#upgrade)
- [Improvements](#improvements)
- [FAQ](#faq)
- [Changelog](#changelog)
- [A reward](#a-reward)
- [Credits](#credits)
- [Support and discussion](#support-and-discussion)


## Configuration

| Specifications | Detail                                                  |
| ------------------- | ------------------------------------------- |
| Computer model      | Xiaomi NoteBook Pro 15.6''(MX150/GTX)      |
| Processor           | Intel Core i5-8250U/i7-8550U Processor     |
| Memory              | 8GB/16GB Samsung DDR4 2400MHz              |
| Hard Disk           | Samsung NVMe SSD Controller PM961/PM981    |
| Integrated Graphics | Intel UHD Graphics 620                     |
| Monitor             | BOE NV156FHM-N61 FHD 1920x1080 (15.6 inch) |
| Sound Card          | Realtek ALC298 (layout-id:30/99)           |
| Wireless Card       | Intel Wireless 8265                        |
| SD Card Reader      | Realtek RTS5129/RTS5250S                   |


## Current Status

- **Ethernet may not work on macOS10.15, view [#256](https://github.com/daliansky/XiaoMi-Pro-Hackintosh/issues/256)**
- In macOS10.15, you need to update [Wireless-USB-Adapter Driver](https://github.com/chris1111/Wireless-USB-Adapter/releases)
  - If you are not using macOS10.15, it's still recommended to update the driver above
- **Discrete graphic card**is not working, since macOS doesn't support Optimus technology
  - Have used [SSDT-DDGPU](ACPI/SSDT-DDGPU.dsl) to disable it in order to save power
- **Fingerprint sensor** is not working
  - Have used [SSDT-USB](ACPI/SSDT-USB.dsl) to disable it in order to save power
- **Intel Bluetooth** may cause sleep problems and does not support some Bluetooth devices
  - View [Work-Around-with-Bluetooth](https://github.com/daliansky/XiaoMi-Pro-Hackintosh/wiki/Work-Around-with-Bluetooth)
- **Intel Wi-Fi (Intel Wireless 8265)** could work by additional configurations
  - Buy a USB Wi-Fi dongle or supported wireless card
  - Use [itlwm](https://github.com/OpenIntelWireless/itlwm) and [HeliPort](https://github.com/OpenIntelWireless/HeliPort) or [AirportItlwm](https://github.com/OpenIntelWireless/itlwm) to drive Intel Wi-Fi
- **Realtek USB SD Card Reader (RTS5129)** is not working
  - Have used [SSDT-USB](ACPI/SSDT-USB.dsl) to disable it in order to save power
- Everything else works well

### Clover
- Support macOS10.13 ~ macOS10.15.6, but **not macOS11+**
- Should Clean NVRAM after using OpenCore
  - Press `Fn+F11` in Clover boot page

### OpenCore
- Support macOS10.13 ~ macOS11.0 beta 5 (20A5354i)
- **Software in Windows may lose activation due to different hardware UUID generated by OpenCore**
  - According to [OpenCore Official Configuration](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/Configuration.pdf), you can try to inject the original firmware UUID to `PlatformInfo - Generic - SystemUUID` in `/OC/config.plist`
- Should Clean NVRAM after using Clover
  - Press `Space` in OpenCore boot page, and then select `Reset NVRAM` entry
- Limited theme
- **Recommend Reading: [OpenCore Configuration](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/Configuration.pdf)**, especially the **UEFISecureBoot** section


## Installation

### First-time installation

- Please refer to the following installation tutorials
  - [Xiaomi Mi Notebook Pro High Sierra 10.13.6](https://www.tonymacx86.com/threads/guide-xiaomi-mi-notebook-pro-high-sierra-10-13-6.242724)
  - [Xiaomi Mi Notebook Pro MacOS Catalina Installation Guide || ENGLISH](https://bit.ly/34biTqw)
- or video tutorials
  - [Xiaomi NoteBook PRO HACKINTOSH INSTALLATION GUIDE !!!](https://www.youtube.com/watch?v=72sPmkpxCvc)
  - [GUIA HACKINTOSH ESPAÑOL 2020 || Instalación de macOS Catalina Xiaomi Mi Notebook Pro](https://www.youtube.com/watch?v=rfG4sGwhE2g)
- If the trackpad doesn't work during the installation, please plug a wired mouse or a wireless mouse projector before the installation. After the installation completes, open `Terminal.app` and run `sudo kextcache -i /`. Wait for the process ending and restart the device. Enjoy your trackpad!
- Complete EFI packs are available in the [releases](https://github.com/daliansky/XiaoMi-Pro-Hackintosh/releases) page.
 - Please don't clone or download the master branch for daily use.
 
 <img src="Docs/img/README_donot_Clone_or_Download.jpg" width="300px" alt="donot_clone_or_download">
 <img src="Docs/img/README_get_Release.jpg" width="300px" alt="get_release">


### Build

- Build the latest beta EFI by running the following command in Terminal:
```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/daliansky/XiaoMi-Pro-Hackintosh/master/makefile.sh)"
```
- Or run the following command in Terminal:
```shell
git clone --depth=1 https://github.com/daliansky/XiaoMi-Pro-Hackintosh.git
cd XiaoMi-Pro-Hackintosh
./makefile.sh
```
- Some advanced usages are:
```shell
# Ignore errors when the script is running
./makefile.sh --IGNORE_ERR
# Bundled with Chinese verison Docs
./makefile.sh --LANG=CN
# Preserve work files during the building stage
./makefile.sh --NO_CLEAN_UP
# Bypass GitHub API
./makefile.sh --NO_GH_API
# Build the latest beta EFI with pre-release kexts
./makefile.sh --PRE_RELEASE=Kext
# Build the latest beta EFI with pre-release OpenCore
./makefile.sh --PRE_RELEASE=OC
```


### Upgrade

- A complete replacement of `BOOT` and `CLOVER`(or `OC`) folders is required. Delete these two folders and copy them from the [release pack](https://github.com/daliansky/XiaoMi-Pro-Hackintosh/releases).
- You can also update Clover EFI by running the following command in Terminal:

```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/daliansky/XiaoMi-Pro-Hackintosh/master/install.sh)"
```


## Improvements

- Use [ALCPlugFix](ALCPlugFix) to fix unworking jack after replug
- Use [itlwm](https://github.com/OpenIntelWireless/itlwm) and [HeliPort](https://github.com/OpenIntelWireless/HeliPort) or [Black80211-Catalina](https://github.com/usr-sse2/Black80211-Catalina) to drive Intel Wi-Fi
- Use [DVMT_and_0xE2_fix](BIOS/DVMT_and_0xE2_fix) to set DVMT to 64mb and unlock CFG
- Use [NVMeFix](https://github.com/acidanthera/NVMeFix) to enable APST on NVMe SSDs
- Use [xzhih](https://github.com/xzhih)'s [one-key-hidpi](https://github.com/xzhih/one-key-hidpi) to improve quality of system UI
  - Support 1424x802 HiDPI resolution
  - On macOS > 10.13.6, to enable higher HiDPI resolution (<=1520x855), you need to use [DVMT_and_0xE2_fix](BIOS/DVMT_and_0xE2_fix) to set DVMT to 64mb first and then change the value for `framebuffer-flags` to `CwfjAA==` in `config.plist - Devices (DeviceProperties) - Properties (Add) - PciRoot(0x0)/Pci(0x2,0x0)`
  - Optional, change `ig-platform-id` to `0x05001c59` (macOS version > 10.14) to enhance graphic performance
- Use [one-key-cpufriend](one-key-cpufriend) to modify CPU power management or change SMBIOS model to `MacBookPro15,4` (macOS version > 10.15)
- Add `igfxrpsc=1` boot-args or `rps-control` property to enable RPS control patch and improves IGPU performance (macOS version ≠ 10.15.6)


## FAQ

#### My touchpad isn't working after update.

You need to rebuild the kext cache after every system update. Use `Kext Utility.app` or type `sudo kextcache -i /` in `Terminal.app`. Then restart. If this still doesn't work, try to press F9.

#### I can't click to drag files using the trackpad.

Starts from [VoodooI2C v2.4.1](https://github.com/alexandred/VoodooI2C/releases/tag/2.4.1), the click down action is emulated to force touch, which causes the failure of click down and drag gestures. You can turn off `Force Click` in `SysPref - Trackpad` or choose `three finger drag` in `SysPref - Accessibility - Mouse & Trackpad - Trackpad Options`.

#### My screen turns to black and has no response during the updating process.

If you have black screen for five minutes and get no response from the device, please force restart your laptop(Long press power button) and choose `Boot macOS Install from ~` entry.

#### My device is locked by `Find My Mac` and can't be booted, what should I do now?

For Clover users, press `Fn+F11` when you are in Clover boot page. Then Clover will refresh `nvram.plist`, and lock message should be removed.  
For OC users, press `Esc` to enter the boot menu during startup. Then, press `Space` key and choose `Reset NVRAM`.

#### [Clover] I opened the `FileVault`, and I can't find macOS partition in Clover boot page, how can I solve it?

It is not recommended to open `FileVault`. You can press `Fn+F3` in the Clover boot page and choose the icon with `FileVault`. Then you can boot in the system and close `FileVault`.

#### [Clover] I can't boot in Windows/Linux by using Clover, but able to boot by press `F12` and select OS.

Many people met this problem by using the new version of `AptioMemoryFix.efi`. A workaround is to delete `AptioMemoryFix.efi` (or `OcQuirks.efi`, `OpenRuntime.efi`, and `OcQuirks.plist`) in `/CLOVER/drivers/UEFI/` and replace it with the old version provided in [#93](https://github.com/daliansky/XiaoMi-Pro-Hackintosh/issues/93).

Also make sure `Sandbox` and `Hyper-V` functions in Windows 10 are disabled.

#### [OC] How to skip the boot menu and automatically boot into the system?

First, in macOS, open `SysPref - Startup Disk`. Choose the target system.  
Then, open `/EFI/OC/config.plist`, and turn off `ShowPicker`.  
When you want to switch OS, press `Esc` during startup to call the boot menu.

#### [OC] How to enable startup chime?

Turn on `AudioSupport` and `PlayChime` in `/OC/config.plist - UEFI - Audio`.

### Please refer to detailed FAQ in [wiki FAQ](https://github.com/daliansky/XiaoMi-Pro-Hackintosh/wiki/FAQ).


## Changelog

You can view [Changelog](Changelog.md) for detailed information.


## A reward

All the project is made for free, but you can reward me if you want.

| Wechat                                                     | Alipay                                               |
| ---------------------------------------------------------- | ---------------------------------------------------- |
| ![wechatpay_160](http://7.daliansky.net/wechatpay_160.jpg) | ![alipay_160](http://7.daliansky.net/alipay_160.jpg) |


## Credits

- Thanks to [Acidanthera](https://github.com/acidanthera) for providing [AppleALC](https://github.com/acidanthera/AppleALC), [AppleSupportPkg](https://github.com/acidanthera/AppleSupportPkg), [HibernationFixup](https://github.com/acidanthera/HibernationFixup), [Lilu](https://github.com/acidanthera/Lilu), [NVMeFix](https://github.com/acidanthera/NVMeFix), [OcBinaryData](https://github.com/acidanthera/OcBinaryData), [OpenCorePkg](https://github.com/acidanthera/OpenCorePkg), [VirtualSMC](https://github.com/acidanthera/VirtualSMC), [VoodooInput](https://github.com/acidanthera/VoodooInput), [VoodooPS2](https://github.com/acidanthera/VoodooPS2), and [WhateverGreen](https://github.com/acidanthera/WhateverGreen).
- Thanks to [apianti](https://sourceforge.net/u/apianti), [blackosx](https://sourceforge.net/u/blackosx), [blusseau](https://sourceforge.net/u/blusseau), [dmazar](https://sourceforge.net/u/dmazar), and [slice2009](https://sourceforge.net/u/slice2009) for providing [Clover](https://github.com/CloverHackyColor/CloverBootloader).
- Thanks to [daliansky](https://github.com/daliansky) for providing [OC-little](https://github.com/daliansky/OC-little).
- Thanks to [FallenChromium](https://github.com/FallenChromium), [jackxuechen](https://github.com/jackxuechen), [Javmain](https://github.com/javmain), [johnnync13](https://github.com/johnnync13), [Menchen](https://github.com/Menchen), [Pasi-Studio](https://github.com/Pasi-Studio), [qeeqez](https://github.com/qeeqez), and [Bat.bat](https://github.com/williambj1) for valuable suggestions.
- Thanks to [hieplpvip](https://github.com/hieplpvip) and [syscl](https://github.com/syscl) for providing sample of DSDT patches.
- Thanks to [OpenIntelWireless](https://github.com/OpenIntelWireless) for providing [IntelBluetoothFirmware](https://github.com/OpenIntelWireless/IntelBluetoothFirmware).
- Thanks to [RehabMan](https://github.com/RehabMan) for providing [EAPD-Codec-Commander](https://github.com/RehabMan/EAPD-Codec-Commander), [EFICheckDisabler](https://github.com/RehabMan/hack-tools/tree/master/kexts/EFICheckDisabler.kext), [OS-X-Clover-Laptop-Config](https://github.com/RehabMan/OS-X-Clover-Laptop-Config), [OS-X-Null-Ethernet](https://github.com/RehabMan/OS-X-Null-Ethernet), and [SATA-unsupported](https://github.com/RehabMan/hack-tools/tree/master/kexts/SATA-unsupported.kext).
- Thanks to [VoodooI2C](https://github.com/VoodooI2C) for providing [VoodooI2C](https://github.com/VoodooI2C/VoodooI2C).

### For more detail, please go to [Reference page](https://github.com/daliansky/XiaoMi-Pro-Hackintosh/wiki/References).


## Support and discussion

- Mi Notebooks supported by other projects:
  - [Mi-Gaming-Laptop](https://github.com/johnnync13/XiaomiGaming) by [johnnync13](https://github.com/johnnync13)
  - [Mi-NB-Air-125-6y30](https://github.com/johnnync13/EFI-Xiaomi-Notebook-air-12-5) by [johnnync13](https://github.com/johnnync13)
  - [Mi-NB-Air-125-7y30](https://github.com/influenist/Mi-NB-Gaming-Laptop-MacOS) by [influenist](https://github.com/influenist)
  - [Mi-NB-Air-133-Gen1](https://github.com/johnnync13/Xiaomi-Notebook-Air-1Gen) by [johnnync13](https://github.com/johnnync13)
  - [Mi-NB-Air-133-2018](https://github.com/johnnync13/Xiaomi-Mi-Air) by [johnnync13](https://github.com/johnnync13)

- tonymacx86.com:
  - [[Guide] Xiaomi Mi Notebook Pro High Sierra 10.13.6](https://www.tonymacx86.com/threads/guide-xiaomi-mi-notebook-pro-high-sierra-10-13-6.242724)

- QQ:
  - 247451054 [小米PRO黑苹果高级群](http://shang.qq.com/wpa/qunwpa?idkey=6223ea12a7f7efe58d5972d241000dd59cbd0260db2fdede52836ca220f7f20e)
  - 137188006 [小米PRO黑苹果](http://shang.qq.com/wpa/qunwpa?idkey=c17e190b9466a73cf12e8caec36e87124fce9e231a895353ee817e9921fdd74e)
  - 689011732 [小米笔记本Pro黑苹果](http://shang.qq.com/wpa/qunwpa?idkey=dde06295030ea1692d6655564e392d86ad874bd0608afd7d408c347d1767981b)
