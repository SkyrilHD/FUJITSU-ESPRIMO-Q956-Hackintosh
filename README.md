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
- [x] Boot
- [x] Ethernet
- [x] GPU Acceleration
- [x] USB
- [x] WiFi/BT

## Known Issues / not working

- [ ] Sleep
- [ ] Hotplug
- [ ] Multi-monitor output

## Download and Install

Go to the [Releases](https://github.com/SkyrilHD/FUJITSU-ESPRIMO-Q956-Hackintosh/releases/) page of this repo and download the latest release according to the macOS you want to install. Then, copy the EFI folder to your EFI partition... That's it.

## Stuck on [EB|#LOG:EXITBS:START]

In the 'Graphics Configuration' section of the BIOS, do not set the 'DVMT Total Graphics Memory Size' to `MAX`. Instead, set it to `256 MB`.

## Error message `OC: Failed to drop ACPI 52414D44 0000000000000000 0 (0) - Not Found` on Boot

OpenCore cannot find the DMAR table because VT-d is disabled. Enable VT-d in the 'CPU Configuration' section of the BIOS.

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

***Security***

**Secure Boot Configuration**

* Secure Boot Control: Disabled

## Credits

Thanks to:

- me (for wasting my time writing this and providing fixes)
- acidanthera (for making an awesome bootloader)
- dortania (for their guide)
- zxystd (for fixing the Bluetooth issue and Intel WiFi card)
