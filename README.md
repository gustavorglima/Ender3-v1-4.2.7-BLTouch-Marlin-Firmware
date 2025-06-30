# Configure Marlin 2.1.2.5 for Ender 3 v1 with Creality v4.2.7 board and BLTouch

This repository contains customized configuration files for the Marlin 3D printer firmware.

### 🖨️ Target Hardware

- **Printer**: Creality Ender 3 v1
- **Board**: Creality V4.2.7 (STM32F103RE)
- **Sensor**: BLTouch
- **Firmware**: Marlin 2.1.2.5

## 📦 Precompiled Firmware

If you don't want to compile the firmware yourself, you can download the precompiled binary:

- **[Download firmware-Marlin-2.1.2.5-Ender3-v1-Creality-v4.2.7-BLTouch.bin](./firmware-Marlin-2.1.2.5-Ender3-v1-Creality-v4.2.7-BLTouch.bin)**

**How to install:**
1. Format a microSD card as FAT32.
2. Copy the `.bin` file to the root of the card.
3. Power off your Ender 3.
4. Insert the card into the printer.
5. Power the printer back on.
6. Wait for it to flash — the screen will stay blank for a few seconds, then boot normally.
7. The file will be automatically deleted after a successful flash.

## 🧩 How to Build

1. **Download Marlin 2.1.2.5**
   - Visit: [https://github.com/MarlinFirmware/Marlin/releases/tag/2.1.2.5](https://github.com/MarlinFirmware/Marlin/releases/tag/2.1.2.5)
   - Download the `Source code (zip)` and extract it.

2. **Copy Configuration Files**
   - Clone or download this repository.
   - Copy all files (`Configuration.h`, `Configuration_adv.h`, `Version.h`, `_Bootscreen.h`, `_Statusscreen.h`) into the `Marlin/` folder inside the extracted Marlin source.

3. **Open VSCode**
   - Open the Marlin root folder in VSCode.
   - Install the [PlatformIO](https://platformio.org/platformio-ide) extension from the VSCode Marketplace.
   - Install the [Auto Build Marlin](https://marketplace.visualstudio.com/items?itemName=MarlinFirmware.auto-build) extension from the VSCode Marketplace.


4. **Build & Upload**
   - In the VSCode sidebar, click the **Auto Build Marlin** icon.
   - Select:
     - **Environment**: `STM32F103RE_creality`
     - **Action**: Build or Upload
   - Monitor the progress and wait for the confirmation.

5. **Flash the Firmware**
   - After building, the compiled firmware file will be located at:
     ```
     .pio/build/STM32F103RE_creality/firmware.bin
     ```
   - Copy the file to the root directory of a **microSD card** (formatted as FAT32).
   - Power off the printer, insert the card, and power it back on.
   - The printer will automatically flash the new firmware and delete the file from the SD card once complete.


### 📚 Reference

- To view the official default configuration for the Ender 3 (Marlin 2.1.2.5), visit:  
  [Official Marlin Ender 3 config](https://github.com/MarlinFirmware/Configurations/tree/release-2.1.2.5/config/examples/Creality/Ender-3)