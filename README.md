# ESP32RC_COTROLLER_PCB-loRa

# 🎮 ESP32-S3 Mini Wireless Transmitter

This project is a custom wireless transmitter designed using the **ESP32-S3 Mini**. It includes an **SPI-based 1.8" LCD**, dual **analog joysticks**, and an **NRF24L01+ module** for RF communication. Powered by a **500mAh Li-Po battery**, it also features an onboard charging circuit based on the **MCP73831**.

## 🧠 Core Features
- ✅ **ESP32-S3 Mini** for powerful wireless and processing capabilities
- 📟 **1.8" SPI TFT LCD** for displaying data (ST7735)
- 🕹️ **Dual analog joysticks** (X, Y + button)
- 📡 **NRF24L01+** module for RF communication
- 🔋 **500mAh Li-Po battery** for portable operation
- ⚡ **MCP73831** Li-Ion charging IC for onboard charging (via USB)

## 📷 Project Preview
*Coming soon – renders or assembled photos*

## 🛠️ Tools & Libraries Used
- 🔧 Designed in **Fusion 360 Electronics**
- 📱 Programmed using **VS Code + PlatformIO**
- 📚 **[RF24 Library](https://github.com/nRF24/RF24)** for NRF24L01+ communication
- 📊 LCD driven by **Adafruit ST7735** or similar display library
- 🛠️ Powered via custom PCB with CH340 and onboard battery management

## 🔌 Wiring Overview
| Component       | Interface   | Notes                       |
|----------------|-------------|-----------------------------|
| TFT LCD         | SPI         | 1.8", uses ST7735 driver    |
| NRF24L01+       | SPI         | 3.3V logic only             |
| Joysticks       | Analog Pins | X, Y, and button lines      |
| Battery         | Li-Po 500mAh| Rechargeable                |
| MCP73831        | Charging IC | Handles safe USB charging   |

## 🧪 PlatformIO Setup

Add the following to your `platformio.ini` file:

```ini
[env:esp32-s3-dev]
platform = espressif32
board = esp32-s3-devkitc-1
framework = arduino
upload_speed = 115200
monitor_speed = 115200
build_flags = 
    -DNRF24L01_ENABLED
lib_deps =
    nRF24/RF24
    adafruit/Adafruit ST7735 and ST7789 Library
```
## 🧠 Part 2: Firmware Logic & Functionality

### 🕹️ Input Reading
- Both analog joysticks are read using `analogRead()`, capturing X and Y axis positions.
- Joystick buttons are handled as digital inputs using internal pull-ups.

---

### 🧾 Data Packet Structure
Joystick values and button states are packaged into a simple `struct` and transmitted via NRF24 using the **RF24** library:

```cpp
struct ControllerPacket {
  uint16_t joyX1;
  uint16_t joyY1;
  uint16_t joyX2;
  uint16_t joyY2;
  bool btn1;
  bool btn2;
};
```
> ⚠️ Don’t forget to `#include <RF24.h>` and initialize SPI correctly for the ESP32-S3!

---

### 📡 RF Transmission

Utilizes the RF24 library with dynamic payloads:

```cpp
radio.begin();
radio.setPALevel(RF24_PA_LOW);
radio.setChannel(115);
radio.openWritingPipe(address);
radio.stopListening();
```

### 🖼️ LCD Display

The 1.8" SPI LCD is used to display joystick values or feedback like battery level or connection status. It is great for debugging or expanding the user interface.

```cpp
tft.initR(INITR_BLACKTAB);
tft.setRotation(1);
tft.setTextColor(ST77XX_WHITE);
tft.setTextSize(1);
```
### 🔋 Power & Charging Notes

- Powered by a 500mAh 3.7V Li-Po
- Charged via MCP73831 IC using USB
- Optional: Status LED for charging indicator
- Includes battery protection diode and capacitor filtering

---

### 🚧 Future Improvements

- 🔋 Battery level sensing via voltage divider
- 📶 RSSI or packet loss display
- 🔐 Add basic encryption or CRC checks
- 🔄 OTA firmware update support
- 🎮 Custom 3D-printed enclosure

