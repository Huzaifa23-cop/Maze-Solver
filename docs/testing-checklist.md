# Testing Checklist

> Complete each step in order. Keep motors disabled until safety checks pass.

## A) Pre-Power Safety

- [ ] Wiring inspected for shorts/loose connections
- [ ] Battery polarity verified
- [ ] Common ground confirmed (battery-, TB6612 GND, ESP32 GND)
- [ ] Buck converter output measured and confirmed safe for ESP32 input path
- [ ] Motor driver VM/VCC/GND wiring verified
- [ ] Emergency stop behavior reviewed in firmware configuration

## B) USB Bring-Up

- [ ] ESP32-S3 boots over USB
- [ ] Serial monitor opens at 115200
- [ ] No boot-loop or repeated reset behavior

## C) Display + Buttons

- [ ] OLED powers and displays expected status text
- [ ] Start/stop button events detected correctly
- [ ] Mode/calibration button events detected correctly

## D) Raw Sensor Validation

- [ ] 8-channel raw sensor values are readable
- [ ] Sensor polarity (white/black response) verified
- [ ] All channels react to line movement

## E) Sensor Calibration

- [ ] Calibration mode enters correctly
- [ ] Min/max values update during sweep
- [ ] Stored calibration appears valid after reboot (if persistent storage used)

## F) MPU6050 (Optional)

- [ ] I2C detection success
- [ ] Gyro/IMU raw values update
- [ ] Basic yaw/turn-assist test completed
- [ ] Drift behavior documented

## G) Motor Driver + Motor Safety Test

- [ ] Wheels lifted before first spin
- [ ] Direction control verified (left/right each direction)
- [ ] PWM speed response verified
- [ ] Stop command halts both motors

## H) PID Line Follow

- [ ] Low-speed straight-line tracking passes
- [ ] Oscillation observed and reduced via tuning
- [ ] PID constants documented per track conditions

## I) Junction Logic

- [ ] Junction detection triggers expected state
- [ ] Left/right/straight decision logic validated
- [ ] Dead-end detection and U-turn behavior validated

## J) Maze Runs

- [ ] Dry run stores path in L/R/S/U sequence
- [ ] Path simplification runs without invalid commands
- [ ] Actual run replays simplified path
- [ ] Goal detection and stop behavior verified

## K) Post-Run

- [ ] Motors stop safely on exit
- [ ] No overheating on motor driver/regulator
- [ ] Notes recorded for next tuning iteration
