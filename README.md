# Hackintosh-Dell-G7-7588-Clover
## Overview
![screenshot](https://cdn.discordapp.com/attachments/780671387878031360/812702686708236288/87855751_p1.png)
Artist: ![Nahaki](https://www.pixiv.net/en/users/9685977)
- macOS: Both Catalina and Big Sur latest version.
- Bootloader: Clover 5130
- EFI can be used for both USB Installer and booting.
- You can use Clover Configurator for configure config.plist file.

## Hardware configuration
* Dell G7 7588
  - CPU: i7-8750H
  - RAM: 2 x 8GB DDR4 (upgraded)
  - Display: NV156FHM 1080p 144Hz (upgraded)
  - SSD: 
      * M.2 NVME Western Digital SN730 256GB for macOS
      * 2.5 inch SATAIII Crucial CT500MX500SSD1 500GB for Windows and Data
  - Sound: ALC256
  - Wireless + Bluetooth: Replaced with DW1560 aka BCM94352Z
  - VGA: Nvidia GTX 1050Ti (disabled)
  - BIOS: 1.15.0 (latest)
## BIOS Configuration
* Recommend you should restore the BIOS setting to Factory Setting first. Then configure the following things:
  * UEFI Boot Path Security: **Never**
  * SATA Operation: **AHCI**
  * Enabled USB Boot Support: **Enabled**
  * Enable External USB Port: **Enabled**
  * Thunderbolt Security: **No Security**
  * Thunderbolt Boot Support: **Enabled**
  * Thunderbolt Auto Switch: **Native Enumeration**
  * PTT Security: **Disabled**
  * Secure Boot Enable: **Disabled**
  * Intel SGX: **Disabled**
  * VT for Direct I/O: **Disabled**
  * Auto OS Recovery Threshold: **Disabled**
  * SupportAssist OS Recovery: **Disabled**

## Graphics
Integrated Intel UHD Graphics 630 support is handled by WhateverGreen, and configured in the `Graphics` section of `config.plist`.
The Nvidia GPU is not supported so it is disabled in SSDT.
The default BIOS DVMT pre-alloc value of `64MB` is sufficient and does not need to be changed.
### Enabling acceleration
* Graphics -> ig-platform-id:
  * `0x3EA50009`
### Fixing backlight registers on CoffeeLake platform
* Boot Argument:
  * `-igfxblr`
### Enabling external display support
* Boot Argument
  * `agdpmod=vit9696`

## Brightness
* Brightness slider and brightness keys are enabled in the DSDT.
* Brightness keys are used with `BRT6 Method`.

## iMessages and Facetime
- Make sure you use `UseMacAddr0` in `RtVariables/ROM`. It is MAC address of Ethernet, and you have to make sure it is `en0`.
- You should get a new valid serial number and other SMBIOS related data for iMessage/Facetime to work, with the SMBIOS is `MacBookPro15,1`. You can generate new one in SMBIOS section.

## Audio
* For ALC256, I use id `56`. Injected in `Devices` section.
* For fixing 3.5mm jack, please go to [Post-Install](https://github.com/rex-lapis/Hackintosh-Dell-G7-7588-Clover#post-install) for more information.

## USB-C
- USB-C port is supported hotplug now! Remember to configure correct BIOS settings.

## Touch ID/Goodix Fingerprint Sensor
* Since I'm using the `MacBookPro15,1` SMBIOS, macOS is expecting Touch ID to be available, causing lag on password prompts. So it can be disabled using `NoTouchID.kext`.

## Keyboard and Trackpad
* The keyboard is PS2, so I use `VoodooPS2Controller.kext`. But I have already deleted `VoodooInput.kext`, `VoodooPS2Trackpad.kext` and `VoodooPS2Mouse.kext` plugins inside because of some stupid things and not need.
* The trackpad is from Synaptics, but it is I2C-HID device. So it can be driven with VoodooI2C, also provides basic trackpad support. For me, I use `VoodooI2C.kext` and `VoodooI2CHID.kext`.

## Power Management, Sleep, Wake and Hibernation
* Hibernation is not supported on a Hackintosh and everything related to it should be completely disabled. Disabling additional features prevents random wakeups while the lid is closed. After every update, these settings should be reapplied manually.
* For disable hibernation, please go to [Post-Install](https://github.com/rex-lapis/Hackintosh-Dell-G7-7588-Clover#post-install) for more information.

## System Integrity Protection (SIP)
* SIP is enabled with macOS Big Sur.

## FileVault
* Not recommended with Clover.

## CFG-Unlock (Highly recommended)
**Note: You should do this on OpenCore**
* Run `modGRUBShell.efi` at OpenCore menu boot screen.
* When `> grub` show up, type `setup_var 0x5BD 0x00`, hit Enter.
* The screen will show `setting offset 0x5bd to 0x00`, that done. Then type `reboot` and hit Enter.
* Now you can disable `KernelXcpm` in `Kernel and Kext Patches` section in Clover Configurator.

## Post-Install
* There is a script file in Post-Install folder. Run it after you're already finished installing macOS. It will help to fix the output and input audio when you plug 3.5mm headphone/headset/external speaker in, and disable hibernation for enhancing sleep.
* Disable hibernation commands in script:
```
sudo pmset -a hibernatemode 0
sudo rm -f /var/vm/sleepimage
sudo mkdir /var/vm/sleepimage
sudo pmset -a standby 0
sudo pmset -a autopoweroff 0
sudo pmset -a powernap 0
sudo pmset -a proximitywake 0
```

## Credit
* Apple for macOS.
* Clover Team for CloverBootLoader.
* Acidanthera Team for many Kernel Extensions.
* Dortania Team for Quirks section for Coffee Lake Laptop guide.
* Ivs1974 for ComboJack Fix

## Support
* Support me ʕ•ᴥ•ʔ☆: [Paypal](https://www.paypal.me/tekun0lxrd)
