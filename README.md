# Hackintosh EFI for Dell Precision 7530 (Coffee Lake) - macOS Sequoia

Professional OpenCore EFI configuration for the **Dell Precision 7530** mobile workstation running macOS Sequoia. This configuration focuses on absolute stability, low power draw, tuned acoustics, and hardware compatibility with 8th Gen Intel Core H-series processors.

## 💻 System Specifications

| Component           | Details                                                                 |
| ------------------- | ----------------------------------------------------------------------- |
| **Model**           | Dell Precision 7530                                                     |
| **CPU**             | Intel Core i7-8850H (6 Cores / 12 Threads) / 8th Gen H-Series          |
| **Integrated GPU**  | Intel UHD Graphics 630 (`0x3EA50009`)                                   |
| **Dedicated GPU**   | NVIDIA Quadro P2000 / P-series / M-series (Disabled via SSDT & `-wegnoegpu`) |
| **RAM**             | DDR4-2666                                                               |
| **Audio**           | Realtek ALC289 / ALC3281 (alcid=11)                                     |
| **Ethernet**        | Intel I219-LM                                                           |
| **Wi-Fi / BT**      | Intel Wireless-AC 9260 / AX200 (using `itlwm` and `IntelBluetooth`)     |
| **Touchpad**        | I2C HID (Alps/Dell) with Multi-touch support & tuned palm rejection     |
| **BIOS Version**    | Latest recommended (A23/A24 range)                                      |
| **SMBIOS**          | `MacBookPro16,1` (Optimized for 8th Gen H-series)                       |

## ✅ What's Working

- [x] **Full Graphics Acceleration** (Intel UHD 630) via WhateverGreen with framebuffer connector patches.
- [x] **External Display Output via USB-C** (Type-C DisplayPort/VGA adapters supported without freezing).
- [x] **Audio** Internal Speakers, Combo Jack (Line-in/OUT) via ALC289/11 (Requires [ComboJack](https://github.com/macos86/ComboJack) for full support).
- [x] **Wi-Fi & Bluetooth** (Intel-based) with AirDrop/Handoff support (note: `itlwm` requires HeliPort).
- [x] **Touchpad & Trackstick** (Multi-touch gestures supported; palm rejection tuned via `AlpsHID.kext` to prevent cursor hopping while typing).
- [x] **Optimized CPU Power & Thermals** (`CPUFriend` configured with an 800 MHz floor and Balanced EPP `0x80`, dropping idle power and fan noise).
- [x] **USB-C Hot-Plug Stability** (Blocked crashing Apple Thunderbolt kexts on stock PC Titan Ridge hardware).
- [x] **Battery Management** (Accurate percentage, cycle count, and health via `VirtualSMC`).
- [x] **Brightness Control** (Slider and Hotkeys working via `SSDT-PNLF`).
- [x] **Sleep & Wake** (Stable native power management).
- [x] **USB Ports** (Fully mapped via `UTBMap.kext` with proper Type-C connectors).
- [x] **SD Card Reader** (via `RealtekCardReader.kext`).
- [x] **NVMe Support** (Optimized via `NVMeFix.kext`).

## ⚠️ What's Not Working / Limitations

- [ ] **NVIDIA Quadro P2000 / P-series / M-series**: Discrete GPU is NOT supported in macOS Mojave and later due to lack of Apple/NVIDIA Web Drivers. It is disabled via `-wegnoegpu` and SSDT to preserve battery life and eliminate excess heat.
- [ ] **Built-in HDMI / Mini-DP Ports**: On the Precision 7530, the physical HDMI and Mini DisplayPort connectors are hardwired to the discrete NVIDIA GPU. Use USB-C video adapters for external monitors.
- [ ] **Native Apple Thunderbolt PCIe Tunneling**: Without hardware-flashing custom firmware onto the Titan Ridge EEPROM with an SPI programmer, macOS's native `IOThunderboltFamily` causes kernel panics (page faults) during hot-plug. Thunderbolt family drivers are safely blocked so USB-C display adapters, hubs, and peripherals hot-plug cleanly without crashing.

## ⚙️ BIOS Settings

To ensure stability, configure your BIOS with the following settings:

### Disable
- [ ] Secure Boot
- [ ] Fast Boot
- [ ] VT-d (Can be enabled if `DisableIoMapper` is on in OpenCore)
- [ ] Wake on LAN
- [ ] Computrace / Absolute Persistence

### Enable
- [x] SATA Mode: **AHCI** (Mandatory)
- [x] UEFI Boot Mode
- [x] Intel Virtualization Technology
- [x] Enable Legacy Option ROMs (if using older BIOS)

### Thermal Management (Dell BIOS)
- **Power Management → Thermal Management**: Set to **Quiet** (for silent fan acoustics during light tasks) or **Optimized** (factory balanced).

## 📦 Post-Installation: Serial Numbers (SMBIOS)

**IMPORTANT**: The `config.plist` in this EFI has been sanitized for public sharing. You **MUST** generate and add your own unique serial numbers before attempting to sign in to Apple ID, iMessage, or iCloud.

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

- **OpenCore 1.0.x**: Clean, modern configuration passing `ocvalidate` with 0 issues.
- **CPU Power Management**: Tailored `CPUFriendDataProvider.kext` for Intel Core i7-8850H (Low Frequency Mode = 800 MHz, EPP = `0x80` Balanced).
- **Thunderbolt Hot-Plug Freeze Fix**: Implemented OpenCore `Kernel -> Block` entries for `com.apple.iokit.IOThunderboltFamily` and `com.apple.driver.AppleThunderboltNHI` to prevent kernel traps / page faults when hot-plugging USB-C accessories.
- **Trackpad Palm Rejection**: Customized `AlpsHID.kext` (`QuietTimeAfterTyping = 500ms`) to eliminate accidental cursor hops while typing.
- **DisplayPort Connector Injection**: Mapped `con1` and `con2` under Intel UHD 630 (`0x3EA50009`) in `DeviceProperties` for USB-C display adapter compatibility.
- **Optimized SSDTs**: 
    - `SSDT-Disable_GPU`: Disables the discrete NVIDIA card.
    - `SSDT-PLUG`: Native CPU power management.
    - `SSDT-PNLF`: Display backlight control.
    - `SSDT-XOSI`: Windows-intended hardware ACPI compatibility.
- **Custom USB Map**: Clean port mapping with `UTBMap.kext` declaring Type-C connectors.
- **Dell Thermal & Sensor Support**: Integrated `SMCDellSensors` with conflicting `SMCSuperIO` disabled.

## 🤝 Credits

- [Acidanthera](https://github.com/acidanthera) for OpenCore, Lilu, WhateverGreen, CPUFriend, and VirtualSMC.
- [OpenIntelWireless](https://github.com/OpenIntelWireless) for itlwm and Bluetooth drivers.
- [VoodooI2C](https://github.com/VoodooI2C) for Touchpad support.
- [Dortania](https://dortania.github.io/OpenCore-Install-Guide/) for documentation.
