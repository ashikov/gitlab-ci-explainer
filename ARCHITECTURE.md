# Architecture

**Status:** initial design. It defines boundaries and invariants, not a mandatory class or module layout.

## Goals

The system should:

- explain supported GitLab CI behavior deterministically;
- separate raw configuration from effective configuration;
- preserve enough provenance to justify derived values and decisions;
- remain useful offline for local features;
- expose unsupported semantics instead of silently approximating them;
- keep the semantic core easy to test without file-system, terminal, or network dependencies.

## Non-goals

The project does not execute job scripts, schedule pipelines, replace GitLab Runner, or validate arbitrary shell commands. Early versions do not need a server, database, plugin system, background processing, or LLM integration.

## Processing model

```text
configuration files + optional pipeline context
                    │
                    ▼
              source loading
                    │
                    ▼
               YAML parsing
                    │
                    ▼
       configuration composition        added in Milestone 2
                    │
                    ▼
          semantic normalization
                    │
                    ▼
            context evaluation           added in Milestone 3
                    │
                    ▼
        explanation / evidence model
                    │
                    ▼
           text or structured output
```

Milestone 1 implements only the smallest useful path: load, parse, normalize jobs/stages/needs, and render a summary.

## Conceptual components

These are responsibilities, not required Python class names.

### Source loading

Reads local content and records its origin. Later it may resolve additional sources through local includes or a GitLab client. Network access must not leak into the semantic core.

### Parsing

Converts YAML into plain data while retaining useful source information when practical. Parsing answers what the document contains; it does not decide GitLab semantics.

### Configuration composition

Resolves includes, inheritance, defaults, and overrides into an effective configuration. Every derived value should be traceable to its source or to a documented default.

### Semantic normalization

Converts flexible YAML forms into a small internal vocabulary: jobs, hidden definitions, stages, dependencies, rules, variables, and diagnostics. Downstream code should not repeatedly interpret every accepted YAML shape.

### Evaluation

Applies an explicit pipeline context to workflow and job-selection rules. Evaluation must return both a result and the evidence that produced it. Unknown or unsupported expressions are distinct from `true` and `false`.

### Explanation

Represents facts such as “the second rule matched because `CI_PIPELINE_SOURCE` equals `merge_request_event`.” Human-readable text is rendered from this evidence; prose is not the source of truth.

### Presentation

Formats results for people or tools. Presentation must not reimplement configuration semantics. Text output should be deterministic so it can be tested and compared.

## Core invariants

1. **No silent fallback.** A construct is supported, invalid, or unsupported. Unsupported behavior is never treated as absent.
2. **Explicit defaults.** Effective values derived from GitLab defaults are distinguishable from values written in YAML.
3. **Provenance survives resolution.** Composition must not discard where a value was defined or overridden.
4. **No execution.** Job scripts and configuration strings are data. The analyzer never evaluates them as shell or Python code.
5. **Pure semantics where practical.** File I/O, terminal formatting, clocks, environment variables, and HTTP clients stay at boundaries.
6. **Stable ordering.** Human-readable output follows a documented deterministic order, independent of hash iteration or concurrency.
7. **Version claims are explicit.** The tool does not claim universal GitLab compatibility.

## Error model

User-visible failures belong to separate categories:

- invocation errors, such as a missing CLI argument;
- source errors, such as a missing or unreadable file;
- syntax errors, such as malformed YAML;
- semantic errors, such as an invalid supported construct;
- unsupported features, where valid GitLab syntax is outside the implemented subset;
- internal errors, which indicate a bug in the tool.

Normal CLI use should not print a traceback for expected user errors. Debug output may be added later without changing the semantic result.

## Compatibility strategy

GitLab CI syntax evolves. Every implemented feature should be grounded in official GitLab documentation and regression fixtures. The repository should eventually record which GitLab version or version range each compatibility profile targets.

Useful primary references:

- [CI/CD YAML syntax reference](https://docs.gitlab.com/ci/yaml/)
- [Configuration includes](https://docs.gitlab.com/ci/yaml/includes/)
- [`needs` dependencies](https://docs.gitlab.com/ci/yaml/needs/)
- [YAML optimization and `extends`](https://docs.gitlab.com/ci/yaml/yaml_optimization/)
- [CI/CD variables](https://docs.gitlab.com/ci/variables/)
- [CI Lint](https://docs.gitlab.com/ci/yaml/lint/)

Differential checks against GitLab may support verification later, but live API calls must not be required for the fast local test suite.

## Security boundaries

Treat every configuration file as untrusted input.

- Use a safe YAML loading mode that cannot construct arbitrary objects.
- Do not execute scripts, expand shell expressions, or import code named by configuration.
- When local includes are added, constrain them to an explicit project root and handle traversal, symlinks, cycles, and duplicate files deliberately.
- Keep remote retrieval disabled until authentication, redirects, size limits, timeouts, and allowed protocols are designed and tested.
- Never print access tokens or secret variable values in diagnostics.

## Testing strategy

Prefer behavior-focused tests:

- small unit tests for normalization and evaluation;
- table-driven cases for alternate YAML forms and edge cases;
- file fixtures for composition and provenance;
- CLI integration tests for exit behavior and rendered output;
- a regression test for every confirmed bug.

Tests should protect observable behavior and domain rules, not private helper names or an accidental module layout. Live GitLab tests, when introduced, should be optional and separate from mandatory local checks.

## Change policy

Use the simplest representation that supports the current milestone. Introduce a new abstraction only when a concrete behavior requires it. Record a short ADR only for decisions that are expensive to reverse, affect compatibility, or change a core invariant.
