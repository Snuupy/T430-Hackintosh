# T430-Hackintosh




In Progress: Sierra (10.12) on a ThinkPad T430
Brightness fix: https://www.tonymacx86.com/threads/guide-lenovo-t430-el-capitan.175935/page-3#post-1132884
https://www.tonymacx86.com/threads/lenovo-t430-mavericks.131428/#post804109


https://github.com/RehabMan/Laptop-DSDT-Patch

## Steps:

Disk Utility -> Erase

	- USB
	- Mac OS Extended (Journaled)
	- GUID Partition Map

Unibeast installation

	- Settings: USB, Sierra, UEFI

Run Clover Installer (Clover_v2.4k_r4061.pkg)

	- Change Install Location: USB Drive
	- Customize: (check the following) 
		- Install for UEFI booting only
		- Install Clover in the ESP
		- Drivers64UEFI > OsxAptioFix2Drv-64
		- Drivers64UEFI > OsxAptioFixDrv-64
		- Themes > BGM (RehabMan's config files use this specific theme)

Copy over kext files, HFSPlus.efi, and config that you need to boot

	- Remove EFI/CLOVER/kexts/10.6, 10.7, 10.8, 10.9, 10.10, leaving just 'Other'
	- copy HFSPlus.efi to EFI/CLOVER/drivers64UEFI (https://github.com/JrCs/CloverGrowerPro/raw/master/Files/HFSPlus/X64/HFSPlus.efi)
	- copy the appropriate config.plist with your screen resolution to EFI/CLOVER/config.plist (you will have to rename it if you get it from here: https://github.com/RehabMan/OS-X-Clover-Laptop-Config)
	- copy the following to EFI/CLOVER/kexts/Other:
		- I COPIED THESE FROM X230: https://github.com/xxx10101xxx/ThinkPad-X230-macOS-10.12.x/tree/master/EFI/CLOVER/kexts/Other
		- FakeSMC.kext (https://www.tonymacx86.com/resources/fakesmc.325/)
		- NullCPUPowerManagement.kext (https://www.tonymacx86.com/resources/nullcpupowermanagement.268/)
		- VoodooPS2Controller (https://github.com/RehabMan/OS-X-Voodoo-PS2-Controller or https://bitbucket.org/RehabMan/os-x-voodoo-ps2-controller/downloads/)

	HDDs:
		- Notes:
      - most systems will work without DataHubDxe-64.efi, but some may require it
		  - you might want "EmuVariableUefi-64.efi", but it would depend on whether native NVRAM works for you (most Skylake hardware has non-functional native NVRAM with OS X/macOS)

Patch SSDT to enable native CPU power management:
	- ssdtPrGen.sh
	- sudo sh ssdtPrGen.sh
	- copy SSDT.aml from ~/Library/ssdtPRGen/ to /EFI/CLOVER/ACPI in your EFI System Partition
	- delete NullCPUPowerManagement.kext in /EFI/CLOVER/kexts/ in the EFI System Partition
	- reboot

















#### Set up shortcuts (adds three-finger-swipe):

macOS unsurprisingly has *very Unix* shortcut key configuration that makes you wonder why something like this is so hard for Microsoft to figure out.

It's legitimately as simple as opening a `System Preferences > Keyboard` dialogue, and manually typing in **ANY** menu bar command you want executed when you type a certain key combination in an application or globally.  The fact that Automator can add its scripts to your menubar under "Services" means you can effectively bind a key combination to do literally anything.

VoodooPS2 also binds three-finger-swipe to `ctrl-cmd-[DIRECTION]`, so we can bind it to back and forward (and whatever you want up and down to do) in our web browser.

Here's a cheatsheet (keep in mind that I already created the "launch terminal" Service in Automator):

![keyboard-shortcuts-menu](http://i.imgur.com/spnR3wt.png)

---

You now have a fully functional Thinkpad T430 Hackintosh to install Google web apps and Microsoft text editors on.  

The ideal scenario for me is having a Unix or Unix-like operating system on my laptop that still has an attractive, user-friendly desktop environment.  macOS fits the bill.  Dual booting Windows offers unlimited versatility, even when I'm not at home with my main Windows computer.

### Quirks:

- If you want to use the trackpoint, you have to set the cursor sensitivity to minimum which effectively renders your touchpad useless.  You can press `PrtSc` to disable the touchpad.
