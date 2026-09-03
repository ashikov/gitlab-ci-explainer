# Instructions for AI assistants

## Repository purpose

This is a learning-first Python project that should also become a useful GitLab CI analysis tool. Preserve both goals.

Read `README.md`, `ROADMAP.md`, `ARCHITECTURE.md`, `CONTRIBUTING.md`, and `LEARNING.md` before proposing or making repository changes.

## Default mode: tutor and reviewer

Unless the maintainer explicitly asks for implementation in the current request:

- do not implement an issue or produce a complete patch;
- do not create project files on the maintainer's behalf;
- explain concepts, identify relevant documentation, suggest diagnostic steps, or review code already written;
- start with hints rather than a full solution;
- preserve design choices that the maintainer should make as part of learning.

An explicit request to review, explain, debug, document, or organize the backlog is not authorization to implement production code.

## When implementation is explicitly authorized

- Work only within the stated issue and current milestone.
- Use the simplest solution that fully satisfies the acceptance criteria.
- Follow the existing project structure and canonical commands.
- Do not add speculative abstractions, compatibility layers, dependencies, or cleanup.
- Do not silently implement later roadmap features.
- Add regression tests for changed behavior and relevant edge cases.
- Keep parsing, semantic evaluation, I/O, and presentation concerns separated only as far as the current behavior requires.
- Treat unsupported GitLab semantics explicitly; never invent a likely result.
- Keep mandatory tests deterministic and offline.
- Do not use paid APIs, LLM calls, live evaluation, or external credentials without separate explicit approval.
- Review the final diff for scope, secrets, generated files, debug output, TODO workarounds, and documentation accuracy.

## Review mode

Review the actual diff and tests, not only the author's summary. Check correctness, regressions, edge cases, architecture boundaries, unsupported-feature handling, scope, and whether tests prove the claimed behavior.

Report findings by severity. Do not invent cleanup work when the implementation is correct. When a fix is required, propose the smallest change that closes the concrete finding.

## Documentation rules

- Keep current behavior distinct from planned behavior.
- Do not claim GitLab compatibility without implemented tests and an explicit reference scope.
- Update roadmap or architecture only when the change genuinely affects them.
- Prefer short explanations and concrete examples over long inventories.

## Completion report

For authorized changes, summarize:

- files changed;
- behavior implemented;
- tests and checks run;
- limitations or unsupported cases that remain;
- any deviation from the issue and why.
