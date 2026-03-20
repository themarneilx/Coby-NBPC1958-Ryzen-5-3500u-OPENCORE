# Coby NBPC1958 Hackintosh (AMD Ryzen 5 3500U)

This repository contains an OpenCore EFI configuration for the Coby NBPC1958 laptop, optimized for macOS Ventura (13.7.7).

## Hardware Specifications

| Component | Specification |
| :--- | :--- |
| **CPU** | AMD Ryzen 5 3500U (Picasso) |
| **iGPU** | AMD Radeon Vega 8 |
| **RAM** | 8GB DDR4 |
| **Storage** | 256GB SATA SSD |
| **Ethernet** | Realtek RTL8111 |
| **Wi-Fi** | Intel Wireless (itlwm) |
| **Bootloader** | OpenCore |
| **SMBIOS** | MacBookPro16,3 |

## Status

### Working
- **Graphics Acceleration**: Supported via NootedRed.kext.
- **SATA Detection**: Fixed using AMDSata.kext with a custom patch for ID 1022:7901.
- **Battery Management**: SMCBatteryManager + ECEnabler.
- **Audio**: AppleALC (alcid=2).
- **Ethernet**: RealtekRTL8111.
- **Input**: Keyboard and Trackpad working in both macOS and OpenCore menu.
- **Power Management**: AMDRyzenCPUPowerManagement + SMCAMDProcessor.

### Fixed in this Version
- **SATA SSD Visibility**: Resolved the issue where internal SATA drives were not appearing in Disk Utility.
- **OpenCore Menu Input**: Keyboard and Trackpad now work in the picker (disabled conflicting PS2 DXE drivers).
- **Kexts**: All drivers updated to their latest 2025/2026 stable releases.

## Installation Notes

### BIOS Settings
Ensure the following are set in your BIOS:
- **SATA Mode**: AHCI (Crucial for drive detection)
- **Secure Boot**: Disabled
- **Fast Boot**: Disabled
- **AMD SVM/Virtualization**: Enabled (optional but recommended)

### Post-Installation
1. **USB Mapping**: Use the USBToolBox tool within macOS to generate a custom UTBMap.kext for better sleep and power efficiency.
2. **Serial Numbers**: This EFI includes a set of serials. For iMessage/FaceTime stability, it is recommended to generate your own using GenSMBIOS.

## Credits
- Acidanthera for OpenCore and core kexts.
- ChefKissInc for NootedRed.
- AlfCraft07 for AMDSata.
- trulyspinach for SMCAMDProcessor.
- Mieze for RealtekRTL8111.
