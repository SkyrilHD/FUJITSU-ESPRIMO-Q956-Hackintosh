# FUJITSU ESPRIMO Q956 Hackintosh

[![OpenCore Version](https://img.shields.io/badge/OpenCore-1.0.5-green.svg)](https://github.com/SkyrilHD/FUJITSU-ESPRIMO-Q956-Hackintosh)
[![GitHub release](https://img.shields.io/github/tag/SkyrilHD/FUJITSU-ESPRIMO-Q956-Hackintosh.svg)](https://github.com/SkyrilHD/FUJITSU-ESPRIMO-Q956-Hackintosh/releases/)
[![GitHub issues](https://img.shields.io/github/issues/SkyrilHD/FUJITSU-ESPRIMO-Q956-Hackintosh.svg)](https://github.com/SkyrilHD/FUJITSU-ESPRIMO-Q956-Hackintosh/issues/)

This repo includes an OpenCore EFI for the FUJITSU ESPRIMO Q956.

Tested on:

Model | ESPRIMO Q956
------------- | ---------------
CPU | Intel Core i5-6500T
GPU | Intel HD Graphics 530
RAM | 12GB DDR4 2133Mhz
Storage | SanDisk SSD Plus 1TB
WiFi | Intel Dual Band Wireless-AC 8260
Software | macOS 15.6.1 Sequoia
BIOS | R1.33.0 (09/29/2021)

## Working features

- [x] Audio
- [x] Boot (learn more about internal boot: [Click](#download-and-install))
- [x] Ethernet
- [x] GPU Acceleration
- [x] USB
- [x] WiFi/BT
- [x] Sleep (learn more: [Click](#sleep))

## Known Issues / not working

- [ ] Hotplug
- [ ] Multi-monitor output
- [ ] Screen blanking on 1440p+ Displays

## Download and Install

Go to the [Releases](https://github.com/SkyrilHD/FUJITSU-ESPRIMO-Q956-Hackintosh/releases/) page of this repo and download the latest release according to the macOS you want to install. Then, copy the EFI folder to your EFI partition... That's it.

Some UEFI firmwares fail to detect the standard fallback bootloader (`EFI/BOOT/BOOTx64.efi`) when installed on an internal drive. As a result, you cannot boot OpenCore directly from the internal disk without manually adding a boot entry. On external drives, the firmware does recognize `BOOTx64.efi`, so OpenCore boots normally. The workaround is to manually create a UEFI boot entry within OpenCore’s built-in shell.

1. Boot from the **external OpenCore USB**.

2. Once in the OpenCore picker, press the spacebar to show extra tools and launch `OpenShell.efi`.

3. In OpenShell, list all partitions with: `map -b`.

4. Find the **internal disk’s EFI partition** with `ls fsX:\` (replace X with the partition number, e.g. fs0:\, fs1:\, etc.).

5. Look for the `EFI` folder that contains `EFI\OC\OpenCore.efi`.

6. Add a UEFI boot entry pointing to OpenCore: `bcfg boot add 3 fsX:\EFI\OC\OpenCore.efi "OpenCore"` (replace X with the correct partition number).

7. Type `exit` to leave OpenShell. This returns you to the OpenCore menu.

8. Shut down, unplug the USB, and reboot. Your system should now boot OpenCore from the internal drive.

## Stuck on `[EB|#LOG:EXITBS:START]`

In the 'Graphics Configuration' section of the BIOS, do not set the 'DVMT Total Graphics Memory Size' to `MAX`. Instead, set it to `256 MB`.

## Error message `OC: Failed to drop ACPI 52414D44 0000000000000000 0 (0) - Not Found` on Boot

OpenCore cannot find the DMAR table because VT-d is disabled. Enable VT-d in the 'CPU Configuration' section of the BIOS.

## Intel WiFi

The Ventura+ EFI includes AirportItlwm for Sonoma 14.4+. To use Intel WiFi on macOS Sequoia and newer, download [HeliPort](https://github.com/OpenIntelWireless/HeliPort/releases). For older macOS versions, replace the AirportItlwm kext according to your version of macOS. Here is the link to AirportItlwm: [Link](https://github.com/OpenIntelWireless/itlwm/releases)

VT-d is supported but disabled in macOS due to issues with the network stack.

## Sleep

Sleep is a known issue with Skylake and Kaby Lake iGPUs. While the system can successfully enter sleep mode, it fails to wake up properly. When attempting to wake up the system, it ends up crashing and rebooting instead of resuming. There is currently no fix or patch available for this behavior. It is a hardware-level compatibility limitation. As a workaround, this EFI uses hibernation to enable sleep. However, due to the nature of hibernation, it takes noticeably longer to wake the computer compared to normal sleep because the system state must be restored from disk instead of being kept in memory.

## Screen blanking after a few seconds

I spent days trying to debug this issue, but I couldn't fix it. I suspect it is a bandwidth issue. Use 1080p for the best results.

<details>

<summary>Here is an excerpt of this issue found in logs</summary>

```
default 12:09:51.287569-0700 kernel IG:: SWInterruptHandler:22897 Pipe Underrun Interrupt
default 12:09:51.287615-0700 kernel [IGFB][ERROR ] CURSOR CTL register values start
default 12:09:51.287617-0700 kernel [IGFB][ERROR ] Pipe0 cursor at address = 0x70080 CursorCtl = 0x0 CursorMode = 0
default 12:09:51.287620-0700 kernel [IGFB][ERROR ] Pipe1 cursor at address = 0x71080 CursorCtl = 0x4000027 CursorMode = 39
default 12:09:51.287622-0700 kernel [IGFB][ERROR ] Pipe2 cursor at address = 0x72080 CursorCtl = 0x0 CursorMode = 0
default 12:09:51.287625-0700 kernel [IGFB][ERROR ] CURSOR CTL register values end
default 12:09:51.287626-0700 kernel [IGFB][ERROR ] PLANE CTL register values start
default 12:09:51.287628-0700 kernel [IGFB][ERROR ] Pipe0 plane0 at address = 0x70180 PLN_CTL = 0x0
default 12:09:51.287630-0700 kernel [IGFB][ERROR ] Pipe0 plane1 at address = 0x70280 PLN_CTL = 0x0
default 12:09:51.287631-0700 kernel [IGFB][ERROR ] Pipe0 plane2 at address = 0x70380 PLN_CTL = 0x0
default 12:09:51.287633-0700 kernel [IGFB][ERROR ] Pipe1 plane0 at address = 0x71180 PLN_CTL = 0xc2081000
default 12:09:51.287636-0700 kernel [IGFB][ERROR ] Pipe1 plane1 at address = 0x71280 PLN_CTL = 0x0
default 12:09:51.287652-0700 kernel [IGFB][ERROR ] Pipe1 plane2 at address = 0x71380 PLN_CTL = 0x0
default 12:09:51.287655-0700 kernel [IGFB][ERROR ] Pipe2 plane0 at address = 0x72180 PLN_CTL = 0x0
default 12:09:51.287657-0700 kernel [IGFB][ERROR ] Pipe2 plane1 at address = 0x72280 PLN_CTL = 0x0
default 12:09:51.287658-0700 kernel [IGFB][ERROR ] Pipe2 plane2 at address = 0x72380 PLN_CTL = 0x0
default 12:09:51.287661-0700 kernel [IGFB][ERROR ] PLANE CTL register values end
default 12:09:51.287775-0700 kernel [IGFB][ERROR ] Plane WM values for pipe0 plane1 at address = 0x70344 level1 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287777-0700 kernel [IGFB][ERROR ] Plane WM values for pipe0 plane1 at address = 0x70348 level2 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287780-0700 kernel [IGFB][ERROR ] Plane WM values for pipe0 plane1 at address = 0x7034c level3 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287781-0700 kernel [IGFB][ERROR ] Plane WM values for pipe0 plane1 at address = 0x70350 level4 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287786-0700 kernel [IGFB][ERROR ] Plane WM values for pipe0 plane1 at address = 0x70354 level5 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287787-0700 kernel [IGFB][ERROR ] Plane WM values for pipe0 plane1 at address = 0x70358 level6 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287792-0700 kernel [IGFB][ERROR ] Plane WM values for pipe0 plane1 at address = 0x7035c level7 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287795-0700 kernel [IGFB][ERROR ] Plane WM values for pipe0 plane2 at address = 0x70440 level0 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287796-0700 kernel [IGFB][ERROR ] Plane WM values for pipe0 plane2 at address = 0x70444 level1 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287800-0700 kernel [IGFB][ERROR ] Plane WM values for pipe0 plane2 at address = 0x70448 level2 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287801-0700 kernel [IGFB][ERROR ] Plane WM values for pipe0 plane2 at address = 0x7044c level3 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287803-0700 kernel [IGFB][ERROR ] Plane WM values for pipe0 plane2 at address = 0x70450 level4 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287806-0700 kernel [IGFB][ERROR ] Plane WM values for pipe0 plane2 at address = 0x70454 level5 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287810-0700 kernel [IGFB][ERROR ] Plane WM values for pipe0 plane2 at address = 0x70458 level6 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287811-0700 kernel [IGFB][ERROR ] Plane WM values for pipe0 plane2 at address = 0x7045c level7 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287813-0700 kernel [IGFB][ERROR ] Plane WM values for pipe1 plane0 at address = 0x71240 level0 : Enabled = 1 Blocks = 0x51 and Lines = 0x0
default 12:09:51.287816-0700 kernel [IGFB][ERROR ] Plane WM values for pipe1 plane0 at address = 0x71244 level1 : Enabled = 1 Blocks = 0xc9 and Lines = 0xa
default 12:09:51.287818-0700 kernel [IGFB][ERROR ] Plane WM values for pipe1 plane0 at address = 0x71248 level2 : Enabled = 1 Blocks = 0xf1 and Lines = 0xc
default 12:09:51.287821-0700 kernel [IGFB][ERROR ] Plane WM values for pipe1 plane0 at address = 0x7124c level3 : Enabled = 1 Blocks = 0x105 and Lines = 0xd
default 12:09:51.287823-0700 kernel [IGFB][ERROR ] Plane WM values for pipe1 plane0 at address = 0x71250 level4 : Enabled = 1 Blocks = 0x1a5 and Lines = 0x15
default 12:09:51.287826-0700 kernel [IGFB][ERROR ] Plane WM values for pipe1 plane0 at address = 0x71254 level5 : Enabled = 1 Blocks = 0x1e1 and Lines = 0x18
default 12:09:51.287827-0700 kernel [IGFB][ERROR ] Plane WM values for pipe1 plane0 at address = 0x71258 level6 : Enabled = 1 Blocks = 0x209 and Lines = 0x1a
default 12:09:51.287831-0700 kernel [IGFB][ERROR ] Plane WM values for pipe1 plane0 at address = 0x7125c level7 : Enabled = 1 Blocks = 0x259 and Lines = 0x1e
default 12:09:51.287832-0700 kernel [IGFB][ERROR ] Plane WM values for pipe1 plane1 at address = 0x71340 level0 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287836-0700 kernel [IGFB][ERROR ] Plane WM values for pipe1 plane1 at address = 0x71344 level1 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287837-0700 kernel [IGFB][ERROR ] Plane WM values for pipe1 plane1 at address = 0x71348 level2 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287841-0700 kernel [IGFB][ERROR ] Plane WM values for pipe1 plane1 at address = 0x7134c level3 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287842-0700 kernel [IGFB][ERROR ] Plane WM values for pipe1 plane1 at address = 0x71350 level4 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287844-0700 kernel [IGFB][ERROR ] Plane WM values for pipe1 plane1 at address = 0x71354 level5 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287847-0700 kernel [IGFB][ERROR ] Plane WM values for pipe1 plane1 at address = 0x71358 level6 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287849-0700 kernel [IGFB][ERROR ] Plane WM values for pipe1 plane1 at address = 0x7135c level7 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287852-0700 kernel [IGFB][ERROR ] Plane WM values for pipe1 plane2 at address = 0x71440 level0 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287856-0700 kernel [IGFB][ERROR ] Plane WM values for pipe1 plane2 at address = 0x71444 level1 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287858-0700 kernel [IGFB][ERROR ] Plane WM values for pipe1 plane2 at address = 0x71448 level2 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287862-0700 kernel [IGFB][ERROR ] Plane WM values for pipe1 plane2 at address = 0x7144c level3 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287864-0700 kernel [IGFB][ERROR ] Plane WM values for pipe1 plane2 at address = 0x71450 level4 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287867-0700 kernel [IGFB][ERROR ] Plane WM values for pipe1 plane2 at address = 0x71454 level5 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287869-0700 kernel [IGFB][ERROR ] Plane WM values for pipe1 plane2 at address = 0x71458 level6 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287872-0700 kernel [IGFB][ERROR ] Plane WM values for pipe1 plane2 at address = 0x7145c level7 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287873-0700 kernel [IGFB][ERROR ] Plane WM values for pipe2 plane0 at address = 0x72240 level0 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287878-0700 kernel [IGFB][ERROR ] Plane WM values for pipe2 plane0 at address = 0x72244 level1 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287879-0700 kernel [IGFB][ERROR ] Plane WM values for pipe2 plane0 at address = 0x72248 level2 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287883-0700 kernel [IGFB][ERROR ] Plane WM values for pipe2 plane0 at address = 0x7224c level3 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287884-0700 kernel [IGFB][ERROR ] Plane WM values for pipe2 plane0 at address = 0x72250 level4 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287887-0700 kernel [IGFB][ERROR ] Plane WM values for pipe2 plane0 at address = 0x72254 level5 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287889-0700 kernel [IGFB][ERROR ] Plane WM values for pipe2 plane0 at address = 0x72258 level6 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287893-0700 kernel [IGFB][ERROR ] Plane WM values for pipe2 plane0 at address = 0x7225c level7 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287894-0700 kernel [IGFB][ERROR ] Plane WM values for pipe2 plane1 at address = 0x72340 level0 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287897-0700 kernel [IGFB][ERROR ] Plane WM values for pipe2 plane1 at address = 0x72344 level1 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287899-0700 kernel [IGFB][ERROR ] Plane WM values for pipe2 plane1 at address = 0x72348 level2 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287902-0700 kernel [IGFB][ERROR ] Plane WM values for pipe2 plane1 at address = 0x7234c level3 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287904-0700 kernel [IGFB][ERROR ] Plane WM values for pipe2 plane1 at address = 0x72350 level4 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287906-0700 kernel [IGFB][ERROR ] Plane WM values for pipe2 plane1 at address = 0x72354 level5 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287909-0700 kernel [IGFB][ERROR ] Plane WM values for pipe2 plane1 at address = 0x72358 level6 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287913-0700 kernel [IGFB][ERROR ] Plane WM values for pipe2 plane1 at address = 0x7235c level7 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287915-0700 kernel [IGFB][ERROR ] Plane WM values for pipe2 plane2 at address = 0x72440 level0 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287918-0700 kernel [IGFB][ERROR ] Plane WM values for pipe2 plane2 at address = 0x72444 level1 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287931-0700 kernel [IGFB][ERROR ] Plane WM values for pipe2 plane2 at address = 0x72448 level2 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287935-0700 kernel [IGFB][ERROR ] Plane WM values for pipe2 plane2 at address = 0x7244c level3 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287940-0700 kernel [IGFB][ERROR ] Plane WM values for pipe2 plane2 at address = 0x72450 level4 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287944-0700 kernel [IGFB][ERROR ] Plane WM values for pipe2 plane2 at address = 0x72454 level5 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287946-0700 kernel [IGFB][ERROR ] Plane WM values for pipe2 plane2 at address = 0x72458 level6 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287950-0700 kernel [IGFB][ERROR ] Plane WM values for pipe2 plane2 at address = 0x7245c level7 : Enabled = 0 Blocks = 0x7 and Lines = 0x1
default 12:09:51.287952-0700 kernel [IGFB][ERROR ] PLANE watermarks values end
default 12:09:51.287954-0700 kernel [IGFB][ERROR ] Display Pipe Underrun occurred on pipe(s) B
```

</details>

## How to Install macOS Sequoia

There are two ways you can install Sequoia:

1. If you have an already working macOS, download the Installer from the App Store and make a bootable Installer with `createinstallmedia` by using this command in Terminal: `sudo /Applications/Install\ macOS\ Sequoia.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume`

2. If you are using Windows, use [macrecovery.py](https://github.com/acidanthera/OpenCorePkg/tree/master/Utilities/macrecovery) from the offical [OpenCore release package](https://github.com/acidanthera/OpenCorePkg/releases/). Follow this [guide](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/windows-install.html) to understand how it works.

After you have created a bootable Installer, mount the EFI partition of the USB thumb drive and copy the EFI folder to the EFI partition. You can then proceed with the installation as usual. After the installation, mount the EFI partition of the installed OS and copy the EFI folder to its partition.

## BIOS settings

First, load your BIOS to the default settings. Then, apply the following BIOS settings:

***Advanced***

**CPU Configuration**

* VT-d: Enabled

* Intel TXT Support: Enabled

**CSM Configuration**

* Launch CSM: Disabled

**Super IO Configuration**

* Serial Port: Disabled

**Graphics Configuration**

* DVMT Shared Memory Size: 64 MB

* DVMT Total Graphics Memory Size: 256 MB

***Security***

**Secure Boot Configuration**

* Secure Boot Control: Disabled

## Screenshot

![About this Mac](https://github.com/user-attachments/assets/e13ac398-d0cf-4fdd-afc9-d81142357620)

## Credits

Thanks to:

- me (for wasting my time writing this and providing fixes)
- acidanthera (for making an awesome bootloader)
- dortania (for their guide)
- zxystd (for fixing Intel Bluetooth and WiFi)
