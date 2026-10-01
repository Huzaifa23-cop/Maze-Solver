# Contributing

Thanks for contributing to **Meshmerize-Maze-Solver-ESP32S3**.

## Ground Rules

- Keep changes modular and easy to review.
- Do not hardcode private credentials, Wi-Fi passwords, or tokens.
- Do not commit build artifacts (for example `.pio/` outputs).
- Keep safety-first behavior (motors disabled on boot/emergency paths) intact.
- Verify GPIO pin mapping for your exact ESP32-S3 board before claiming wiring correctness.

## Getting Started

1. Fork and clone the repository.
2. Open in VS Code with PlatformIO extension.
3. Build locally (`pio run`) before submitting changes.
4. If hardware-related logic changes are made, document test method and limits.

## Pull Request Guidelines

- Describe what changed and why.
- Include test notes (build result, hardware simulation/manual checks done).
- Keep PRs focused; avoid unrelated refactors.
- Update documentation when behavior/setup changes.

## Issue Reporting

Use the provided issue templates:
- Bug report
- Feature request
