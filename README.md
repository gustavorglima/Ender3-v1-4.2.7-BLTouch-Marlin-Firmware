# Configure Marlin 2.1.2.5 for Ender 3 v1 — Creality v4.2.7 with BLTouch

This repository contains customized configuration files for the Marlin 3D printer firmware.

## 🧩 How to Use with Marlin Firmware Auto Build (VSCode Extension)

This project is intended to be used with the **Marlin Firmware Auto Build** extension for VSCode, which simplifies compiling and uploading firmware to your printer.

### 🔧 Steps

1. **Download Marlin 2.1.2.5**
   - Visit: [https://github.com/MarlinFirmware/Marlin/releases/tag/2.1.2.5](https://github.com/MarlinFirmware/Marlin/releases/tag/2.1.2.5)
   - Download the `Source code (zip)` and extract it.

2. **Copy Configuration Files**
   - Clone or download this repository.
   - Copy all files (`Configuration.h`, `Configuration_adv.h`, `Version.h`, `_Bootscreen.h`, `_Statusscreen.h`) into the `Marlin/` folder inside the extracted Marlin source.

3. **Open VSCode**
   - Open the Marlin root folder in VSCode.
   - Make sure the [PlatformIO extension](https://platformio.org/platformio-ide) is installed.
   - Install the **Marlin Firmware Auto Build** extension from the VSCode Marketplace.

4. **Build & Upload**
   - In the VSCode sidebar, click the **Marlin Auto Build** icon.
   - Select:
     - **Environment**: `STM32F103RE_creality`
     - **Action**: Build or Upload
   - Monitor the progress and wait for the confirmation.

### 🖨️ Target Hardware

- **Printer**: Creality Ender 3 v1
- **Board**: Creality V4.2.7 (STM32F103RE)
- **Sensor**: BLTouch
- **Firmware**: Marlin 2.1.2.5

---
Only configuration and branding files are tracked in this repository. All core Marlin source files are excluded via `.gitignore`.