# Meshmerize Maze Solver Robot — ESP32-S3

An autonomous white-line-following robot that explores a maze in a dry run, records turns, simplifies its route, and replays the learned path during actual run.

> **Project status:** Under active development. Hardware testing, calibration, and tuning are ongoing.

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
| TB6612FNG dual DC motor driver | Drive left/right motors |
| 2x N20 6V 600 RPM motors (no encoders) | Robot propulsion |
| RoboJunkies 8-channel analog IR array | Line and junction detection |
| MPU6050 IMU (optional) | Turn-assistance / heading support |
| SSD1306 0.96" I2C OLED | Menu/debug display |
| Buttons (start/stop, mode/calibration) | User control |
| Red LED | Goal indication |
| 2S 7.4V battery + buck converter | Power system |

## Software Stack

- VS Code
- PlatformIO
- Arduino framework
- ESP32-S3

## Architecture

```mermaid
flowchart LR
    S[Sensors] --> E[ESP32-S3]
    E --> L[PID and Maze Logic]
    L --> D[TB6612FNG]
    D --> M[Motors]
```

## Repository Tree

```text
.
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.md
│   │   └── feature_request.md
│   └── pull_request_template.md
├── docs/
│   ├── images/
│   │   └── .gitkeep
│   ├── maze-algorithm.md
│   ├── testing-checklist.md
│   └── wiring.md
├── CHANGELOG.md
├── CONTRIBUTING.md
├── LICENSE   # to be added after license choice
├── README.md
└── .gitignore
```

## Wiring Summary

> GPIO mappings are **board-dependent**. Treat all pin mappings as **"verify for your ESP32-S3 board."**

| Link | Notes |
|---|---|
| ESP32-S3 ↔ TB6612FNG | PWM + direction + standby/control lines (verify GPIO map) |
| ESP32-S3 ↔ RoboJunkies sensor | 8 analog channels (verify ADC-capable GPIO map) |
| ESP32-S3 ↔ OLED | Shared I2C bus (SDA/SCL verify per board) |
| ESP32-S3 ↔ MPU6050 | Shared I2C bus (same SDA/SCL as OLED) |
| Battery ↔ TB6612 VM | Motor supply path |
| Battery ↔ buck converter ↔ ESP32 5V/VIN | Regulated ESP32 supply path |

## Electrical Safety Warnings

- ESP32-S3 GPIO is **3.3V only**.
- Do **not** feed 5V analog sensor signals directly into ESP32 ADC pins.
- Do **not** connect a 2S battery directly to ESP32 3V3/5V/VIN unless through a correct regulator path for your board.
- Battery negative, TB6612 GND, and ESP32 GND must share a **common ground**.
- TB6612 **VM** is motor power; TB6612 **VCC** is logic supply.
- Confirm buck converter output is **5.0V** with a multimeter before connecting to ESP32 VIN/5V input.
- Follow safe LiPo/Li-ion charging, handling, and storage practices.

## Setup

1. Install VS Code.
2. Install the PlatformIO extension.
3. Open this project folder in VS Code.
4. Connect ESP32-S3 using a USB **data** cable.
5. Build the project.
6. Upload the project.
7. Open Serial Monitor at `115200` baud.

## PlatformIO Commands

```bash
pio run
pio run -t upload
pio device monitor
```

## Recommended Testing Order

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

- **Dry run:** Robot explores and records decisions as `L/R/S/U` at decision points.
- **Actual run:** Robot replays a simplified learned path at higher speed.
- **Decision encoding:**
  - `L` = left
  - `R` = right
  - `S` = straight
  - `U` = U-turn/backtrack
- **Why simplification works well:** It is most effective on branch/dead-end/tree-like mazes where backtracking paths can be reduced.
- **Looped mazes note:** Global shortest path in looped mazes usually needs exploration of relevant alternate routes and graph-aware logic.

## Known Limitations

- Normal N20 motors do not have encoders.
- MPU6050 yaw can drift.
- Sensor calibration depends on surface and lighting.
- GPIO mapping must be confirmed for the exact ESP32-S3 board.
- Track-specific PID tuning may be required.

## Future Upgrades

- Encoder-based motors
- Better buck converter
- Custom PCB
- Graph/BFS exploration for looped mazes
- Data logging
- Improved chassis and sensor mounting

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

License selection is pending your choice (MIT recommended for beginner-friendly open source).
