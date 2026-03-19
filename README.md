# Hackintosh EFI for Dell Precision 7530 (Coffee Lake) - macOS Sequoia

Professional OpenCore EFI configuration for the **Dell Precision 7530** mobile workstation running macOS. This configuration focuses on stability and performance with the 8th Gen Intel H-series processors.

## 💻 System Specifications

| Component           | Details                                                                 |
| ------------------- | ----------------------------------------------------------------------- |
| **Model**           | Dell Precision 7530                                                     |
| **CPU**             | Intel Core i7-8850H (6 Cores / 12 Threads) / 8th Gen H-Series          |
| **Integrated GPU**  | Intel UHD Graphics 630                                                  |
| **Dedicated GPU**   | NVIDIA Quadro M3000M (Disabled via SSDT & boot-args for power saving)   |
| **RAM**             | DDR4-2666                                                               |
| **Audio**           | Realtek ALC289 / ALC3281 (alcid=11)                                     |
| **Ethernet**        | Intel I219-LM                                                           |
| **Wi-Fi / BT**      | Intel Wireless-AC 9260 / AX200 (using `itlwm` and `IntelBluetooth`)     |
| **Touchpad**        | I2C HID (Alps/Dell) with Multi-touch support                           |
| **BIOS Version**    | Latest recommended (A23/A24 range)                                      |
| **SMBIOS**          | `MacBookPro16,1` (Optimized for 8th Gen H-series)                       |

## ✅ What's Working

- [x] **Full Graphics Acceleration** (Intel UHD 630) via WhateverGreen.
- [x] **Audio** Internal Speakers, Combo Jack (Line-in/OUT) via ALC289/11 (Requires [ComboJack](https://github.com/macos86/ComboJack) for full support).
- [x] **Wi-Fi & Bluetooth** (Intel-based) with AirDrop/Handoff support (note: `itlwm` requires HeliPort).
- [x] **Touchpad & Trackstick** (Multi-touch gestures supported).
- [x] **Battery Management** (Accurate percentage and health).
- [x] **Brightness Control** (Slider and Hotkeys working).
- [x] **Sleep & Wake** (Stable native power management).
- [x] **USB Ports** (Fully mapped for optimal power and speed).
- [x] **SD Card Reader** (via `RealtekCardReader.kext`).
- [x] **NVMe Support** (Optimized via `NVMeFix.kext`).

## ⚠️ What's Not Working / Limitations

- [ ] **NVIDIA Quadro M3010M/P-series/M3000M**: Discrete GPU is NOT supported in macOS Mojave and later due to lack of Web Drivers. It is disabled to improve battery life and reduce heat.
- [ ] **HDMI/DisplayPort Out**: These ports are often wired to the NVIDIA GPU. If your model has this configuration, external digital video might not work without the dGPU enabled (which is unsupported).

## ⚙️ BIOS Settings

To ensure stability, configure your BIOS with the following settings:

### Disable
- [ ] Secure Boot
- [ ] Fast Boot
- [ ] VT-d (Can be enabled if `DisableIoMapper` is on in OpenCore)
- [ ] Wake on LAN
- [ ] Computrace / Any form of data security

### Enable
- [x] SATA Mode: **AHCI** (Mandatory)
- [x] UEFI Boot Mode
- [x] Intel Virtualization Technology
- [x] Enable Legacy Option ROMs (if using older BIOS)

## 📦 Post-Installation: Serial Numbers (SMBIOS)

**IMPORTANT**: The `config.plist` in this EFI has been sensitized for public sharing. You **MUST** generate and add your own unique serial numbers before attempting to sign in to iMessage or iCloud.

### Fields to Replace
In `EFI/OC/config.plist` -> `PlatformInfo` -> `Generic`:
- `SystemSerialNumber`: Currently `REPLACEME_SN`
- `MLB`: Currently `REPLACEME_MLB`
- `SystemUUID`: Currently `REPLACEME_UUID`

### How to Generate
1. Use [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) by Corpnewt.
2. Select your `config.plist` and choose `MacBookPro16,1` as the model.
3. Generate new serials and save the changes.

---

## 🛠 Features of this EFI

- **OpenCore 1.0.x**: Latest stable configurations.
- **Optimized SSDTs**: 
    - `SSDT-Disable_GPU`: Disables the discrete NVIDIA card.
    - `SSDT-PLUG`: For native CPU power management.
    - `SSDT-PNLF`: For backlight support.
    - `SSDT-XOSI`: For better hardware compatibility on Windows-intended hardware.
- **Custom USB Map**: Efficient power delivery and sleep behavior.
- **Dell-Specific Sensors**: Monitors fans and temperatures via `SMCDellSensors`.

## 🤝 Credits

- [Acidanthera](https://github.com/acidanthera) for OpenCore, Lilu, WhateverGreen, and VirtualSMC.
- [OpenIntelWireless](https://github.com/OpenIntelWireless) for itlwm and Bluetooth drivers.
- [VoodooI2C](https://github.com/VoodooI2C) for Touchpad support.
- [Dortania](https://dortania.github.io/OpenCore-Install-Guide/) for the amazing documentation.
