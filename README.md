# Configure Marlin 2.1.2.5 for Ender 3 v1 with Creality v4.2.7 board and BLTouch

🇧🇷 Versão em português: [README.pt-BR.md](./README.pt-BR.md)

## 📑 Table of Contents

- [🎯 Target Hardware](#️-target-hardware)
- [📦 Precompiled Firmware](#-precompiled-firmware)
- [🔧 Z-Offset and Auto Bed Leveling Calibration (BLTouch)](#-z-offset-and-auto-bed-leveling-calibration-bltouch)
- [🧰 Sending Commands via USB](#-sending-commands-via-usb)
- [🧩 How to Build](#-how-to-build)
- [📚 Reference](#-reference)

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

After flashing the firmware, [calibrate the Z-offset and bed leveling](#-z-offset-and-auto-bed-leveling-calibration-bltouch).

## 🔧 Z-Offset and Auto Bed Leveling Calibration (BLTouch)

After flashing the firmware, it's important to calibrate the Z-offset and bed leveling mesh:

1. **Reset all settings to default**:
   ```
   M502 ; Reset settings
   M500 ; Save settings
   M501 ; Load saved settings
   ```

2. **Home all axes**:
   ```
   G28
   ```

3. **Move to the center of the bed**:
   ```
   G1 X107 Y107 Z5 F6000
   ```

4. **Lower the nozzle until it touches a piece of paper**:
   - Use small Z movements (`G1 Z-0.05`, etc.) until there's light friction on the paper.

5. **Read the current position and set Z-offset**:
   ```
   M114 ; Note the Z value (e.g., Z:-2.35)
   M851 Z-2.35 ; Replace with your value
   M500 ; Save to EEPROM
   ```

6. **Generate and save the bed leveling mesh**:
   ```
   G28    ; Home again
   G29    ; Run mesh auto-leveling
   M500   ; Save mesh to EEPROM
   ```

7. **Enable mesh leveling on every print**:
   Ensure your slicer start G-code includes:
   ```
   G28
   M420 S1
   ```

💡 If you haven't flashed the firmware yet, see [Precompiled Firmware](#-precompiled-firmware) or [How to Build](#-how-to-build).

## 🧰 Sending Commands via USB

To send G-code commands like `G28`, `G29`, or `M851`, you can use free software that communicates with your printer over a USB cable.

### Recommended Software

- **Pronterface (Printrun)**  
  Download: [https://github.com/kliment/Printrun](https://github.com/kliment/Printrun)  
  A simple interface to send commands and control the printer manually.

- **OctoPrint**  
  Website: [https://octoprint.org](https://octoprint.org)  
  A full-featured web interface for controlling your printer remotely (requires Raspberry Pi or PC setup).

- **Repetier-Host**  
  Website: [https://www.repetier.com/download-now/](https://www.repetier.com/download-now/)  
  Windows/Linux/macOS support with easy USB connectivity and console access.

### How to Connect via USB

1. Plug your Ender 3 into your computer via a USB cable (usually USB-B or USB Mini-B).
2. Open the software (e.g., Pronterface).
3. Select the correct COM port (usually shown automatically).
4. Set the baud rate to `115200`.
5. Click "Connect".
6. Once connected, you can manually enter commands like:

These are useful for [Z-offset calibration](#-z-offset-and-auto-bed-leveling-calibration-bltouch).

   ```
   G28
   G29
   M114
   M851 Z-2.35
   M500
   ```

## 🧩 How to Build

1. **Download Marlin 2.1.2.5**
   - Visit: [https://github.com/MarlinFirmware/Marlin/releases/tag/2.1.2.5](https://github.com/MarlinFirmware/Marlin/releases/tag/2.1.2.5)
   - Download the `Source code (zip)` and extract it.

2. **Copy Configuration Files**
   - Clone or download [this repository](#configure-marlin-2125-for-ender-3-v1-with-creality-v427-board-and-bltouch).
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

🔼 [Back to top](#configure-marlin-2125-for-ender-3-v1-with-creality-v427-board-and-bltouch)