# Testing Checklist

Use this checklist in order. Keep motors disabled unless the current step requires motion.

## A) Pre-Power Safety Checks

- [ ] Wiring inspected for shorts/loose connections
- [ ] Battery polarity verified
- [ ] TB6612 VM and VCC paths reviewed
- [ ] Common ground confirmed (battery/TB6612/ESP32/sensors)
- [ ] Buck converter output measured and set to safe target voltage
- [ ] Battery positive **not** connected directly to ESP32 3V3

## B) Development Boot and UI

- [ ] ESP32-S3 boots over USB without brownout/reset loop
- [ ] Serial monitor opens at 115200
- [ ] OLED initializes and displays expected test output
- [ ] Start/stop and mode/calibration buttons read correctly

## C) Sensor Bring-Up

- [ ] Raw 8-channel sensor values print correctly
- [ ] Sensor channels respond to black/white transitions
- [ ] ADC readings are stable enough for thresholding

## D) Sensor Calibration

- [ ] Min/max values recorded per sensor channel
- [ ] Calibration routine repeatable across runs
- [ ] Normalized/weighted line position output validated

## E) MPU6050 (Optional)

- [ ] I2C detection successful
- [ ] Gyro calibration routine completes
- [ ] Yaw output trend verified (drift noted if present)

## F) Motor Safety and Basic Motion

- [ ] Wheels lifted before first motor spin test
- [ ] TB6612 standby/enable logic verified
- [ ] Left/right motors rotate in intended directions
- [ ] Stop command halts both motors reliably

## G) PID Tuning

- [ ] Start with low speed and conservative gains
- [ ] Tune P term for line capture response
- [ ] Add D to reduce oscillation
- [ ] Add I only if persistent bias remains
- [ ] Confirm stability on straight and curved sections

## H) Maze Behavior

- [ ] Junction detection works (L/R/S candidates)
- [ ] Dead-end handling triggers safe backtrack/U-turn
- [ ] Dry-run path logging stores L/R/S/U sequence correctly
- [ ] Path simplification output reviewed and valid
- [ ] Actual-run replay follows simplified route reliably
- [ ] Goal/end-box handling triggers expected state/action

## I) Emergency and Fault Handling

- [ ] Emergency stop command/input always overrides motion
- [ ] Invalid sensor conditions enter safe error-stop behavior
- [ ] Recovery/reset process documented and repeatable
