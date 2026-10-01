# Wiring and Power Guide (ESP32-S3 Maze Solver)

> All pin numbers are configurable and board-dependent. Verify your exact ESP32-S3 board pinout before final wiring.

## 1) Power Distribution (Concept)

```text
2S Battery (+/-)
   ├─> TB6612 VM (motor voltage input)
   └─> Buck Converter IN (+/-)
          └─> Buck Converter OUT (regulated 5.0V)
                 └─> ESP32-S3 VIN/5V input (board-dependent)

Common Ground Requirement:
Battery (-) = TB6612 GND = ESP32 GND = Sensor/OLED/MPU6050 GND
```

## 2) USB Development / Testing Setup

- Use USB data cable to power/program ESP32-S3 during development.
- Keep motor power path controlled and test motors only when intended.
- During early tests, keep wheels lifted.

## 3) Standalone Battery Setup

- Battery powers motors through TB6612 `VM`.
- Battery also feeds buck converter input.
- Buck converter output (verified) powers ESP32 input rail.
- **Never** connect raw battery positive directly to ESP32 `3V3`.

## 4) TB6612 Signal and Power Distinction

- `VM`: motor supply voltage input.
- `VCC`: logic supply for driver control interface.
- `GND`: must tie to system common ground.
- `AIN1/AIN2/PWMA` and `BIN1/BIN2/PWMB` connect to ESP32 GPIO/PWM (board-dependent).
- `STBY` should be controlled so motors can remain disabled for safe boot.

## 5) Sensor and I2C Devices

- RoboJunkies 8-channel analog sensor outputs connect to ESP32 ADC-capable pins (board-dependent).
- SSD1306 OLED and MPU6050 usually share I2C bus (SDA/SCL).
- Confirm voltage compatibility of all modules with ESP32-S3 logic levels.

## 6) Critical Safety Notes

- ESP32-S3 GPIO is 3.3V logic.
- Do not feed 5V analog lines directly into ESP32 ADC.
- Verify buck converter output at 5.0V with a multimeter before connecting ESP32 VIN/5V path.
- Confirm correct polarity before power-on.
- Re-check common ground before debugging unstable behavior.
