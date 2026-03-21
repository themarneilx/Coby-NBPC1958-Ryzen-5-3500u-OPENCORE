# Coby NBPC1958 Hackintosh (AMD Ryzen 5 3500U)

This repository contains an OpenCore EFI configuration for the Coby NBPC1958 laptop, supporting macOS Ventura (13.x) and macOS Sequoia (15.x).

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
- **USB Mapping**: USBMap.kext generated for this machine (MacBookPro16,3-XHC0).

### Fixed in this Version
- **SATA SSD Visibility**: Resolved the issue where internal SATA drives were not appearing in Disk Utility.
- **OpenCore Menu Input**: Keyboard and Trackpad now work in the picker (disabled conflicting PS2 DXE drivers).
- **Kexts**: All drivers updated to their latest 2025/2026 stable releases.
- **macOS Sequoia Support**: Updated AMD kernel patches from AMD-OSX/AMD_Vanilla for kernel 24.x (Sequoia). Added missing Sequoia-specific patches for `_cpuid_set_generic_info` (leaf7 check) and `_mtrr_update_action` (PAT fix).
- **SecureBootModel**: Set to `Disabled` — required for AMD hackintosh on Sequoia. The previous `Default` value caused Phase 2 installer failure.
- **USB Mapping**: Custom `USBMap.kext` included, generated with corpnewt's USBMap tool.

## Installation Notes

### BIOS Settings
Ensure the following are set in your BIOS:
- **SATA Mode**: AHCI (Crucial for drive detection)
- **Secure Boot**: Disabled
- **Fast Boot**: Disabled
- **AMD SVM/Virtualization**: Enabled (optional but recommended)

### Installing macOS Sequoia

1. Create a macOS Sequoia USB installer using `createinstallmedia` on a Mac or fetch the full installer via `mist-cli` / `macadmins python`.
2. Copy this EFI folder to the ESP (EFI partition) of your USB installer.
3. Boot the USB on the Coby. In the OpenCore picker, select **"macOS Installer"**.
4. Complete Phase 1 of the installer (the USB environment). The machine will reboot.
5. **Keep the USB plugged in.** In the OpenCore picker after reboot, select **"macOS Installer"** again (this is Phase 2, loading from the internal SSD).
6. Phase 2 will complete and reboot one more time into the macOS Setup Assistant.

### Post-Installation

1. **Serial Numbers**: This EFI includes a set of serials. For iMessage/FaceTime stability, generate your own using [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) and replace the values in `PlatformInfo > Generic` in config.plist.
2. **Wi-Fi**: itlwm requires [HeliPort](https://github.com/OpenIntelWireless/HeliPort) to connect to Wi-Fi networks. Install HeliPort and launch it from the menu bar.
3. **Install OpenCore to internal SSD** (optional but recommended so you don't need the USB to boot):
   - Mount the internal SSD's EFI partition with [MountEFI](https://github.com/corpnewt/MountEFI).
   - Copy this EFI folder to the internal SSD's EFI partition.
   - You can then boot without the USB drive.

### Regenerating USB Map (if you replace the machine or re-install)

The included `USBMap.kext` was generated specifically for this machine. If you need to redo it:

1. Remove `USBMap.kext` from `EFI/OC/Kexts/` and its entry from `config.plist`.
2. Boot into macOS.
3. Download [corpnewt's USBMap](https://github.com/corpnewt/USBMap) tool, run it in Terminal.
4. Plug a USB device into every port on the laptop one at a time so the tool detects them.
5. Export the generated `USBMap.kext` and add it to `EFI/OC/Kexts/` and `config.plist`.

## Credits
- Acidanthera for OpenCore and core kexts.
- ChefKissInc for NootedRed.
- AlfCraft07 for AMDSata.
- trulyspinach for SMCAMDProcessor.
- Mieze for RealtekRTL8111.
- AMD-OSX for AMD_Vanilla kernel patches.
- corpnewt for USBMap and GenSMBIOS.
