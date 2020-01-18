# Hackintosh-OpenCore-GA-Z77X-UD5H-3770K-HD4000

### OpenCore EFI folder and support files for the following hardware:

* Motherboard: Gigabyte GA-Z77X-UD5H, rev. 1.1, [BIOS F16j](https://www.gigabyte.com/us/Motherboard/GA-Z77X-UD5H-rev-11/support#support-dl-bios). [Manual](https://download.gigabyte.com/FileList/Manual/mb_manual_ga-z77x-ud5h_e.pdf)
* Processor: Intel Core i7 3770K,
* Graphics: Integrated Intel HD4000
* Wifi/BT: Fenvi FV-T919 (Broadcom BCM94360CD)

Current version is [OpenCore 0.5.4-DEBUG](https://github.com/acidanthera/OpenCorePkg/releases) built with [OCBuilder](https://github.com/Pavo-IM/ocbuilder/releases)

config.plist was modified by following the [OpenCore Vanilla Desktop Guide](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/) and the Configuration.pdf included in OpenCore Docs folder.

This EFI folder is working for my build but you should not expect it to work for you 100%, even with the same hardware. For example, small BIOS setting changes may change behavior & depending on which ports your internal USB headers are plugged into on the motherboard will change how the SSDT-UIAC.aml works. It's important to understand what all the files and config settings do so please use this folder as a reference and build your own following the docs described above.

### ISSUES:

* I'm not able to boot into the Recovery partition. Still trying to find a solution.
* OpenCore has a [known bug](https://github.com/acidanthera/bugtracker/issues/669) causing the onboard Marvell 88SE9172 SATA Controller not work correctly. If any drives are connected to this controller, OpenCore will stall after "connecting drivers" step and take up to 20 minutes to reach picker menu. The controller appears as "Generic AHCI Controller" in SystemInformation. This controller works in Clover so there must be a solution. Affects my ASM1062 PCI SATA controller as well. For now my boot drives are connected to the Intel 6Gb/s SATA ports (SATA 0/1, white ports on motherboard)

### UPDATES:

* WiFi, Bluetooth, & USB PortMap: I recently changed from an old TP-Link wifi card (Atheros AR9380) that was acting glitchy and required an injected kext, to a Fenvi FV-T919 (Broadcom BCM94360CD) for better native MacOS support. The T919 works out of the box and enables some nice features like Handoff and Airdrop from iOS. Unfortunately I started experiencing an issue where the computer would immediately wake up after I put it to sleep (an issue I didn't have with my old D-Link BT2.1 dongle). Also, since the T919 Bluetooth connects to the MOBO via internal USB header, adding this card put me over the 15-port USB limit so I was forced to use USBInjectAll.kext. This kext slows down boot time and I've read it is not an ideal long term solution so I decided to follow [Rehabman's excellent guide to USB port mapping](https://www.tonymacx86.com/threads/guide-creating-a-custom-ssdt-for-usbinjectall-kext.211311/) Now I have a good working SSDT-UIAC.aml file that enables all back panel ports, two front panel USB3 ports, and the Bluetooth card. I have disabled USBInjectAll.kext in my config but it remains in the kext folder in case it's needed in the future. I had to set the internal Blutooth port's 'USBConnector' type to 255 in SSDT-UIAC.aml and uncheck "Allow Bluetooth devices to wake this computer" (System Preferences > Bluetooth > Advanced) to solve the sleep issue. Before the new Bluetooth card, I found no USB mapping necessary to run all the back-panel USB and two front-panel USB3 ports.

* DSDT: I ran with no DSDT for a long time (thanks to Gigabyte's good hackintosh compatibility) but decided to make a modified DSDT (with [MaciASL](https://github.com/acidanthera/MaciASL/releases)) to change a couple of parameters that helped with making the USB PortMap (EHC1 > EH01, EHC2 > EH02, XHCI > XHC, XHC1 > XHC). I found [this guide](http://www.macbreaker.com/2014/03/how-to-edit-your-own-dsdt-with-maciasl.html) useful.


### GUIDE:

I will add more here later:

SSDT-EC will need to be uniquely generated for your CPU if not running an i7 3770K. Use [PikerAlpha's ssdtGen script](https://github.com/Piker-Alpha/ssdtPRGen.sh)

The config is currently set to heavy logging on screen and to file. This slows down the boot time signifcantly. I will remove logging commands once I solve a couple of issues.

Please let me know if you find bugs or enhancements!

### CREDITS:

Acidanthera for [OpenCore](https://github.com/acidanthera/OpenCorePkg/releases), [MaciASL](https://github.com/acidanthera/MaciASL/releases)\
Kronokernel for the [OpenCore Vanilla Desktop Guide](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/)\
Pavo-IM for [OCBuilder](https://github.com/Pavo-IM/ocbuilder/releases)\
RehabMan for the [USB port mapping guide](https://www.tonymacx86.com/threads/guide-creating-a-custom-ssdt-for-usbinjectall-kext.211311/)\
corpnewt for [ProperTree](https://github.com/corpnewt/ProperTree)\
Piker-Alpha for [ssdtGen.sh](https://github.com/Piker-Alpha/ssdtPRGen.sh)\
MaLd0n for help with DSDT patches and useful guides on the [Olarila.com Forum](https://olarila.com/forum/)\

### OTHER RESOURCES:

[Reddit Hackintosh](reddit.com/r/hackintosh)\
[tonymacx86 forum](https://www.tonymacx86.com/forums/)\
[InsanelyMac OSx86 Project Forum](https://www.insanelymac.com/forum/85-osx86-project/)\
