# Maze Algorithm Overview

This project uses a practical competition-oriented strategy:
- **Dry run**: explore and record turns.
- **Path simplify**: reduce unnecessary backtracking moves.
- **Actual run**: replay simplified route faster.

## State Machine

- `SAFE_BOOT` - Initialize safely with motors disabled.
- `SENSOR_CALIBRATION` - Acquire/confirm sensor calibration data.
- `LINE_FOLLOW` - Track white line with PID control.
- `JUNCTION_DECISION` - Detect intersections/dead-ends/end zones and choose direction.
- `TURN` - Execute left/right/U-turn/straight transition.
- `DRY_RUN` - Store exploration sequence (`L/R/S/U`).
- `PATH_SIMPLIFY` - Compress route by removing reversible detours.
- `ACTUAL_RUN` - Replay optimized sequence at higher speed.
- `GOAL_REACHED` - Stop and indicate success.
- `ERROR_STOP` - Emergency or fault stop.

## Decision Encoding

- `L` = left turn
- `R` = right turn
- `S` = straight
- `U` = U-turn / backtrack

These symbols are appended in dry run and later simplified.

## Dead-End Backtracking

When no valid forward branch exists:
1. Mark as dead-end.
2. Execute U-turn (`U`).
3. Return to previous branch point.
4. Continue exploration.

## Path Simplification Idea

Local sequences with immediate backtracking can be reduced.
Example concept:
- Branch taken then reversed can be collapsed when equivalent direct decision is known.
- This works best in branch/dead-end (tree-like) mazes.

## Limitation in Looped Mazes

In looped mazes, locally simplified paths are not guaranteed globally shortest unless relevant alternative loops are discovered and compared. A graph-based approach (e.g., BFS on discovered nodes/edges) is preferred for true global shortest-path guarantees.

## Simple Pseudocode

```text
state = SAFE_BOOT
path = []

loop:
  if emergency: state = ERROR_STOP

  switch state:
    SAFE_BOOT:
      disable_motors()
      init_devices()
      state = SENSOR_CALIBRATION

    SENSOR_CALIBRATION:
      calibrate_sensors()
      state = DRY_RUN

    DRY_RUN:
      while not goal:
        follow_line_pid()
        if junction_or_dead_end():
          move = choose_move_L_R_S_or_U()
          path.append(move)
          execute_turn(move)
      state = PATH_SIMPLIFY

    PATH_SIMPLIFY:
      path = simplify(path)
      state = ACTUAL_RUN

    ACTUAL_RUN:
      for move in path:
        follow_line_pid_fast()
        execute_turn(move)
      state = GOAL_REACHED

    GOAL_REACHED:
      stop_motors()
      signal_goal()
      break

    ERROR_STOP:
      stop_motors()
      signal_error()
      break
```
