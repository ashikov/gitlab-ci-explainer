# Roadmap

The roadmap is ordered. Each milestone should leave the project usable at a new level without requiring unfinished work from later milestones.

Later milestones are intentionally described at a higher level. They should be split into implementation issues only after the previous milestone validates the underlying model.

## Milestone 1 — First useful local CLI (`0.1`)

**Goal:** inspect one local `.gitlab-ci.yml` file and print a trustworthy structural summary.

Implementation is tracked in [Milestone 1: first useful local CLI](/ashikov/gitlab-ci-explainer/issues/2).

Planned work:

1. Establish a minimal Python package, test runner, linting, and the `ci-explain` command.
2. Load a local YAML file and distinguish malformed input from file-system errors.
3. Classify top-level entries as global configuration, runnable jobs, or hidden job definitions.
4. Parse stage order and determine each job's effective stage.
5. Normalize supported same-pipeline `needs` forms.
6. Render stable human-readable output.
7. Define user-facing errors and exit behavior.
8. Add end-to-end fixtures and document the working CLI.

The milestone is complete when a fresh clone can run a documented command against representative valid and invalid fixtures, and every supported result is covered by automated tests.

Out of scope: configuration composition, rule evaluation, GitLab API calls, remote content, graph rendering, and exact emulation of every GitLab version.

## Milestone 2 — Resolve local configuration (`0.2`)

**Goal:** explain the effective configuration assembled from files and reusable job definitions stored in one repository.

Planned capabilities:

- local `include` resolution with cycle and path-boundary protection;
- deterministic GitLab-style merge behavior;
- hidden job definitions and `extends` chains;
- `default` and `inherit` handling;
- provenance for inherited and overridden values;
- explicit diagnostics for unsupported include kinds or merge constructs.

The milestone is complete when the tool can show both a resolved value and where that value came from.

## Milestone 3 — Explain job selection (`0.3`)

**Goal:** answer why a job is included, excluded, manual, delayed, or allowed to fail for a supplied pipeline context.

Planned capabilities:

- an explicit pipeline context such as source, branch, tag, variables, and changed files;
- variable layers and documented precedence;
- a focused subset of `rules:if` expressions;
- `rules:changes` and `rules:exists` where the required local evidence is available;
- first-match rule evaluation with evidence;
- `workflow:rules` before job-level evaluation;
- explicit treatment of deprecated `only` and `except` syntax.

Unsupported expressions must produce an `unsupported` result rather than an invented decision.

## Milestone 4 — Explain execution dependencies (`0.4`)

**Goal:** make stage and DAG behavior easy to inspect.

Planned capabilities:

- a normalized graph derived from stages and `needs`;
- missing-dependency and cycle diagnostics;
- optional and artifact-related dependency metadata;
- queries for one job, its prerequisites, and its dependants;
- deterministic text and machine-readable output;
- optional DOT export after the internal graph proves stable.

## Milestone 5 — Integrate with GitLab (`0.5`)

**Goal:** analyze real repositories without making the deterministic core depend on the network.

Planned capabilities:

- explicit GitLab compatibility profiles;
- project, remote, template, and component includes where feasible;
- GitLab API access behind a narrow boundary;
- authentication, caching, timeouts, and secret-safe logging;
- optional comparison with GitLab CI Lint as verification evidence;
- fixtures captured from real-world configurations without committing secrets.

Local analysis remains available without credentials or network access.

## Milestone 6 — Stable release (`1.0`)

**Goal:** publish a small, dependable tool with a documented compatibility contract.

Release criteria:

- stable CLI and output contracts;
- a supported Python-version policy;
- an explicit GitLab feature and version matrix;
- packaging and installation documentation;
- regression coverage for supported semantics;
- security review of file and network boundaries;
- a selected open-source license;
- migration notes for any pre-`1.0` breaking changes.

## Deferred ideas

These are not roadmap commitments:

- interactive TUI or web UI;
- editor integrations;
- pull request or merge request comments;
- risk scoring for pipeline changes;
- natural-language summaries generated from an already computed explanation model.

An LLM must never become the source of truth for parsing, resolution, or rule evaluation.
