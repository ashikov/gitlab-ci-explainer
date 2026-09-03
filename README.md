# gitlab-ci-explainer

> Explain why GitLab CI jobs run, skip, and depend on each other.

`gitlab-ci-explainer` is a local-first Python CLI for turning GitLab CI configuration into deterministic, inspectable explanations.

> [!IMPORTANT]
> **Status: pre-alpha.** The repository currently contains the project design and implementation backlog. There is no working CLI yet.

## Why

A pipeline can be valid and still be difficult to understand:

- Why did a job appear or disappear?
- Which `rules` entry matched?
- Where did an effective variable value come from?
- Which `include`, default, or parent job changed the final configuration?
- Why can one job start immediately while another waits for an entire stage?

Reading raw YAML is not enough once configuration composition and evaluation start interacting. This project aims to expose the intermediate facts and the reason behind each result instead of guessing or producing an opaque verdict.

## Planned interface

The first useful version is intentionally small. Given one local file:

```yaml
stages: [build, test, deploy]

build:
  stage: build
  script: make build

test:
  stage: test
  needs: [build]
  script: make test

deploy:
  stage: deploy
  needs: [test]
  script: ./deploy.sh
```

it should produce a stable summary similar to:

```console
$ ci-explain summary .gitlab-ci.yml
JOB     STAGE    NEEDS
build   build    -
test    test     build
deploy  deploy   test
```

This is target UX, not current output.

## First milestone

Version `0.1` will only:

- read one local YAML file;
- classify global configuration, runnable jobs, and hidden job definitions;
- determine effective stages;
- normalize same-pipeline `needs` dependencies;
- print a deterministic text summary;
- report invalid or unsupported input clearly.

Not included in `0.1`: `include`, `extends`, `rules` evaluation, GitLab API access, remote content, a web service, or AI-generated explanations.

## Principles

**Deterministic core.** The same input and context must produce the same result.

**Evidence before prose.** An explanation should be built from parsed facts and provenance, not generated as a plausible story.

**Local first.** Network access is added only when a feature truly requires it.

**Explicit compatibility.** Unsupported GitLab features must be reported, not silently ignored.

**Simple design.** No plugin framework, service layer, database, or speculative abstractions before there is a concrete need.

**Learning first.** The maintainer writes the implementation. AI is primarily a tutor, research assistant, debugger, and reviewer.

## Documentation

- [Roadmap](ROADMAP.md) — ordered milestones and release boundaries.
- [Architecture](ARCHITECTURE.md) — system boundaries, processing model, and design constraints.
- [Contributing](CONTRIBUTING.md) — development and pull request workflow.
- [Learning mode](LEARNING.md) — how to use AI without outsourcing the programming practice.
- [AI assistant instructions](AGENTS.md) — repository rules for coding agents.

Implementation starts in the [Milestone 1 tracker](/ashikov/gitlab-ci-explainer/issues/2). Work through its issues in order and keep each pull request limited to one task.

## Project boundary

This tool is not intended to execute job scripts, replace GitLab Runner, or become a second CI system. It should explain the configuration GitLab would use within an explicitly documented compatibility scope.
