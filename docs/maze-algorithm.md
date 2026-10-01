# Maze Algorithm Overview

This project uses a practical two-pass strategy:

1. **Dry run:** Explore and record route decisions (`L/R/S/U`).
2. **Actual run:** Replay a simplified route faster.

## State Machine

- `SAFE_BOOT` — initialize peripherals, keep motors disabled.
- `SENSOR_CALIBRATION` — gather sensor min/max and thresholds.
- `LINE_FOLLOW` — PID tracking on white line.
- `JUNCTION_DECISION` — detect available exits and pick direction.
- `TURN` — execute left/right/U-turn/straight transition.
- `DRY_RUN` — exploration mode with path recording.
- `PATH_SIMPLIFY` — remove unnecessary backtracking moves.
- `ACTUAL_RUN` — high-speed replay of simplified path.
- `GOAL_REACHED` — stop and indicate completion.
- `ERROR_STOP` — fail-safe state for abnormal conditions.

## Decision Storage (L/R/S/U)

At each meaningful decision point:

- `L` = left turn
- `R` = right turn
- `S` = straight
- `U` = U-turn / backtrack at dead-end

The sequence is appended during dry run and then simplified before replay.

## Dead-End Backtracking

When no forward branch is available, the robot performs a controlled U-turn (`U`) and returns to the previous junction, where a different unexplored branch can be chosen.

## Path Simplification

Simplification removes redundant branch/backtrack combinations so actual run is shorter and faster.

This works best in branch/dead-end/tree-like maze structures where backtracking artifacts are easy to cancel.

## Limitation in Looped Mazes

In looped mazes, local simplification alone may not guarantee the global shortest path. To guarantee shortest path, the robot must discover and compare relevant alternate routes (graph-based exploration such as BFS/weighted variants).

## Simple Pseudocode

```text
state = SAFE_BOOT
path_raw = []
path_simplified = []

while true:
  switch state:
    SAFE_BOOT:
      disable_motors()
      init_peripherals()
      state = SENSOR_CALIBRATION

    SENSOR_CALIBRATION:
      calibrate_line_sensors()
      state = DRY_RUN

    DRY_RUN:
      follow_line_pid_low_speed()
      if junction_detected:
        move = choose_direction_policy()
        path_raw.append(move)   # L/R/S/U
        execute_turn(move)
      if dead_end_detected:
        path_raw.append('U')
        execute_turn('U')
      if goal_detected:
        state = PATH_SIMPLIFY

    PATH_SIMPLIFY:
      path_simplified = simplify(path_raw)
      state = ACTUAL_RUN

    ACTUAL_RUN:
      replay(path_simplified, higher_speed=true)
      if goal_detected:
        state = GOAL_REACHED

    GOAL_REACHED:
      disable_motors()
      signal_goal()
      break

    ERROR_STOP:
      disable_motors()
      signal_error()
      wait_for_manual_reset()
```
