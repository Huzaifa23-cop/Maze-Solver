# Wiring and Power Guide (ESP32-S3 + TB6612FNG)

> ⚠️ Pin assignments are board-dependent. Treat GPIO mappings as configurable and verify on your exact ESP32-S3 board.

## 1) Power Distribution (Concept)

```text
2S Battery (+/-)
   ├─> TB6612 VM (motor voltage input) and GND
   └─> Buck Converter IN (+/-)
           └─> Buck Converter OUT (regulated) -> ESP32 5V/VIN and GND

ESP32 GND, TB6612 GND, Sensor GND, OLED GND, MPU6050 GND -> common ground
```

## 2) USB Development / Testing Setup

- Power ESP32-S3 through USB during firmware flashing and serial debugging.
- Keep common ground with motor driver/sensors during integrated testing.
- For first bring-up, keep motor supply disconnected or motors lifted off ground.

## 3) Standalone Battery Setup

- Battery feeds TB6612 VM (motor power).
- Battery also feeds buck converter input.
- Buck converter output feeds ESP32 5V/VIN (or board-recommended power pin).
- **Never connect battery positive directly to ESP32 3V3.**

## 4) TB6612 VM / VCC / GND Distinction

- **VM**: motor power rail (from battery).
- **VCC**: logic supply rail (from controller logic voltage).
- **GND**: shared ground reference.
- STBY, PWM, and direction pins connect to ESP32 GPIO (configurable).

## 5) I2C Sharing (OLED + MPU6050)

- OLED SSD1306 and MPU6050 can share the same I2C SDA/SCL bus.
- Ensure both modules use compatible voltage levels and proper pull-ups.
- Use one consistent I2C pin pair configured in firmware for your board.

## 6) Sensor Array and ADC Caution

- The RoboJunkies analog IR array outputs must be voltage-compatible with ESP32 ADC inputs.
- ESP32-S3 ADC pins are 3.3V-limited.
- If sensor outputs can exceed 3.3V, use appropriate level conditioning before ESP32.

## 7) Safety Checks Before Powering Motors

- Verify buck output with multimeter (target 5.0V for VIN/5V input path as applicable).
- Confirm polarity before connecting battery.
- Confirm all grounds are common.
- Keep wheels lifted for first motor rotation tests.
