# Contributing

Thanks for your interest in improving this project.

## Getting Started

1. Fork the repository.
2. Create a feature branch.
3. Make focused, testable changes.
4. Build locally with PlatformIO before submitting.
5. Open a pull request using the PR template.

## Contribution Guidelines

- Keep changes modular and easy to review.
- Do not commit build outputs (`.pio/`) or secrets.
- Do not hardcode private credentials in source files.
- For hardware mappings, clearly mark board-dependent values.
- Update documentation when behavior or wiring assumptions change.

## Commit and PR Tips

- Use clear commit messages.
- Keep one logical change per PR when possible.
- Include test steps and observed results in the PR description.

## Safety and Validation

- Prefer safe default states (motors disabled at boot).
- Validate motor tests with wheels lifted first.
- Re-check regulator voltage and common ground during hardware tests.
