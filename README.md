# Meshmerize Maze Solver Robot — ESP32-S3

An autonomous white-line-following robot that explores a maze in a dry run, records turns, simplifies its route, and replays the learned path during actual run.

> **Status:** 🚧 Under active development. Hardware testing, calibration, and tuning are still ongoing.

## Features

- 8-channel analog IR line sensing
- PID line following
- TB6612FNG + N20 motors
- Dry-run maze exploration
- L/R/S/U path storage
- Path simplification
- Fast actual-run replay
- OLED menu/debug interface
- Optional MPU6050 turn assistance
- Emergency-stop design

## Hardware

| Component | Purpose |
|---|---|
| ESP32-S3 development board | Main controller |
| TB6612FNG dual DC motor driver | Motor driving |
| 2x N20 6V 600 RPM motors (no encoders) | Drive system |
| RoboJunkies 8-channel analog IR array | White-line sensing |
| MPU6050 (optional) | Turn-assistance/gyro experiments |
| SSD1306 0.96" I2C OLED | Status + debug display |
| Buttons (start/stop, mode/calibration) | UI/control |
| Red goal LED | Goal indicator |
| 2S 7.4V battery + buck converter/regulator | Power system |

## Software Stack

- VS Code
- PlatformIO
- Arduino framework
- ESP32-S3 platform

## Architecture

```mermaid
flowchart LR
  A[Sensors] --> B[ESP32-S3]
  B --> C[PID / Maze Logic]
  C --> D[TB6612FNG]
  D --> E[Motors]
```

## Repository Tree

```text
.
├── .github/
│   ├── ISSUE_TEMPLATE/
│   └── pull_request_template.md
├── docs/
│   ├── images/
│   ├── maze-algorithm.md
│   ├── testing-checklist.md
│   └── wiring.md
├── CHANGELOG.md
├── CONTRIBUTING.md
├── LICENSE                  # Add after choosing license type
├── README.md
└── .gitignore
```

## Wiring Summary (verify for your ESP32-S3 board)

> GPIO mapping is board-dependent. Treat all pin mappings as **configurable** and verify against your exact ESP32-S3 dev board pinout.

| Link | Summary |
|---|---|
| ESP32-S3 ↔ TB6612FNG | PWM + direction + standby control pins from ESP32-S3 (board-dependent) |
| ESP32-S3 ↔ RoboJunkies IR sensor | 8 analog sensor channels to ESP32 ADC-capable pins (voltage compatibility must be verified) |
| ESP32-S3 ↔ OLED (SSD1306) | Shared I2C bus (SDA/SCL, board-dependent) |
| ESP32-S3 ↔ MPU6050 | Same I2C bus as OLED (SDA/SCL, board-dependent) |
| Battery ↔ TB6612 VM | Battery motor power to TB6612 VM input |
| Battery ↔ buck converter ↔ ESP32 5V/VIN | Regulated supply path for controller power |

## Electrical Safety Warnings

- ESP32-S3 GPIO is **3.3V only**.
- Do **not** feed 5V analog sensor signals directly into ESP32 ADC.
- Do **not** connect a 2S battery directly to ESP32 3V3/5V/VIN unless through a correct regulator.
- Battery negative, TB6612 GND, and ESP32 GND must share a common ground.
- TB6612 **VM = motor power**, **VCC = logic supply**.
- Confirm buck converter output is 5.0V with a multimeter before ESP32 connection.
- Follow LiPo/Li-ion charging and handling safety practices.

## Setup

1. Install VS Code.
2. Install the PlatformIO extension.
3. Open this project folder in VS Code.
4. Connect ESP32-S3 using a USB data cable.
5. Build project.
6. Upload project.
7. Open Serial Monitor at `115200`.

### Commands

```bash
pio run
pio run -t upload
pio device monitor
```

## Testing Order

1. USB boot check
2. OLED/button test
3. Raw sensor test
4. Sensor calibration
5. MPU6050 test
6. Motor test with wheels lifted
7. Low-speed PID line follow
8. Junction turn test
9. Dry run
10. Actual run

## Maze Algorithm Summary

- **Dry run:** Robot explores, makes local decisions at junctions, and stores turns as `L/R/S/U`.
- **Actual run:** Robot replays the simplified path faster after exploration.
- **L/R/S/U decisions:** Represent left, right, straight, and U-turn/backtrack actions.
- **Why simplification works:** Best on branch/dead-end/tree-like mazes because backtracked branches can be reduced.
- **Looped mazes limitation:** True global shortest path in looped mazes may require discovering and comparing alternate routes beyond single-pass local simplification.

See `/docs/maze-algorithm.md` for full details and pseudocode.

## Known Limitations

- Normal N20 motors do not have encoders.
- MPU6050 yaw can drift over time.
- Sensor calibration depends on surface and lighting.
- GPIO mapping must be confirmed for the exact board.
- Track-specific PID tuning may be required.

## Future Upgrades

- Encoder motors
- Better buck converter
- Custom PCB
- Graph/BFS exploration for looped mazes
- Data logging
- Improved chassis/sensor mounting

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) before opening issues or pull requests.

## License

License selection is pending. Choose one before first public release:
- MIT
- GPL-3.0
- CERN-OHL-S
- No license (copyright by default)
