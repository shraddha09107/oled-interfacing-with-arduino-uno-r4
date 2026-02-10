# OLED Interfacing with Arduino Uno R4 (SSD1306, I2C)

This small PlatformIO project demonstrates how to drive an I2C SSD1306 128x64 OLED display from an Arduino Uno R4 (WiFi) using the Adafruit GFX and Adafruit SSD1306 libraries.

Contents
- `src/main.cpp` — example sketch that initializes the display and prints three lines of text.
- `platformio.ini` — project configuration (board: `uno_r4_wifi`, libraries, serial/upload ports).

Features
- Uses I2C (Wire) interface to talk to an SSD1306 OLED
- Prints simple static text to the display

Hardware
- Arduino Uno R4 (WiFi) or compatible Renesas RA-based Uno R4 board
- SSD1306 128x64 OLED display (I2C)
- 4 jumper wires

Wiring (SSD1306 -> Uno R4)
- VCC  -> 5V (or 3.3V depending on your module; check module label)
- GND  -> GND
- SDA  -> SDA (on Uno R4 this is the dedicated SDA pin)
- SCL  -> SCL (on Uno R4 this is the dedicated SCL pin)

Notes
- The example sketch uses I2C address `0x3C`. If your module uses a different address (e.g. `0x3D`) update `OLED_ADDR` in `src/main.cpp`.
- Some modules require 3.3V on VCC; consult your display's documentation.

Software / Libraries
- PlatformIO (project already configured in `platformio.ini`)
- Libraries declared in `platformio.ini` (PlatformIO will install them automatically):
  - `adafruit/Adafruit GFX Library`
  - `adafruit/Adafruit SSD1306`

Quick build & upload (PowerShell / PlatformIO)
- Build the project:

```powershell
platformio run -e uno_r4_wifi
```

- Upload to the board (uses `upload_port` from `platformio.ini`):

```powershell
platformio run -e uno_r4_wifi -t upload
```

- Open the serial monitor (9600 baud is set in `src/main.cpp` / `platformio.ini`):

```powershell
platformio device monitor -e uno_r4_wifi --baud 9600
```

Troubleshooting
- "OLED not found" printed to Serial:
  - Check wiring (SDA/SCL, VCC, GND).
  - Confirm I2C address. Run an I2C scanner sketch to list addresses. If the address differs, update `OLED_ADDR` in `src/main.cpp`.
  - Make sure the display's supply voltage matches your module (3.3V vs 5V).
  - Ensure libraries are installed (PlatformIO should install them automatically on first build).

I2C scanner sketch (quick test)
```cpp
#include <Wire.h>

void setup() {
  Wire.begin();
  Serial.begin(9600);
  while (!Serial);
  Serial.println("I2C Scanner");

  for (byte address = 1; address < 127; address++) {
    Wire.beginTransmission(address);
    byte error = Wire.endTransmission();
    if (error == 0) {
      Serial.print("Found I2C device at 0x");
      Serial.println(address, HEX);
    }
  }
}

void loop() {}
```

Project notes / next steps
- Replace the static text with dynamic content (sensors, menus, animations).
- Add a dedicated display helper class for drawing UI elements.
- Add unit tests or CI if the project grows.

License
- This repository contains example code; feel free to reuse and adapt. Add a license file if needed for your project.

If you'd like, I can also:
- Add an I2C scanner as a separate `src` example file,
- Add a small wiring diagram image placeholder, or
- Extend `src/main.cpp` with an example animation or menu.

Requirements coverage:
- Create README.md — Done


