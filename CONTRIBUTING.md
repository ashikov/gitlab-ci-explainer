# Contributing

This repository is both a real tool and a deliberate Python practice project. Correctness matters, but the maintainer's understanding is part of the result.

## Before starting

1. Read [README.md](README.md), [ROADMAP.md](ROADMAP.md), and [ARCHITECTURE.md](ARCHITECTURE.md).
2. Pick the first unblocked issue in the current milestone.
3. Restate the required behavior and edge cases before choosing an implementation.
4. Keep the pull request limited to that issue.

Do not implement later roadmap features “while already in the area.” A smaller complete change is preferred to a broader speculative design.

## Development loop

For each behavior:

1. Add or identify a test that fails for the right reason.
2. Implement the simplest complete solution.
3. Make the relevant tests pass.
4. Refactor only when the tests are green and the change improves the current code.
5. Run the repository's canonical checks.
6. Review the final diff for accidental files, secrets, debug output, unrelated cleanup, and unsupported claims.

The first implementation issue will establish the Python environment and canonical commands. Once those commands exist, document one normal path for setup, tests, linting, formatting, and the full local check. Avoid overlapping toolchains that solve the same problem.

## Tests

Every issue should cover its observable acceptance criteria. Add regression tests for bugs and edge cases that could silently change an explanation.

Prefer compact fixtures that demonstrate one domain rule. Do not copy a large production pipeline into the repository when a minimal example proves the same behavior.

Mandatory tests must be deterministic, offline, and free of credentials. Live GitLab checks belong in a separate optional workflow when they become necessary.

## Commits and pull requests

Use focused branches such as:

```text
issue-12-normalize-needs
```

Use Conventional Commits, for example:

```text
feat: normalize same-pipeline needs
fix: reject non-mapping pipeline root
test: cover hidden job classification
```

Keep commits understandable and avoid mixing independent changes. A pull request should explain:

- the behavior added or changed;
- the main design decision;
- edge cases considered;
- tests and commands run;
- what the author learned or still finds unclear.

The pull request template contains the expected structure.

## Documentation

Update documentation when behavior, scope, commands, output, compatibility, or architecture changes. Do not claim support for a feature based only on successful YAML parsing; support means the relevant semantics are implemented and tested.

## AI assistance

Follow [LEARNING.md](LEARNING.md) and [AGENTS.md](AGENTS.md). AI may explain concepts, locate primary documentation, suggest experiments, diagnose a traceback, and review a patch. The default is not to hand an entire issue to a coding agent.

External paid APIs, LLM-backed features, or live evaluation runs require explicit maintainer approval before use.
