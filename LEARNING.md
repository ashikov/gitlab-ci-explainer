# Learning mode

The project exists partly to restore hands-on Python skill. Shipping a tool written mostly by an agent would satisfy the product goal and fail the learning goal.

## Ownership rule

The maintainer owns the first implementation attempt for each issue: reading the requirement, choosing data structures, writing code, running tests, and debugging the first failure.

AI is primarily used as:

- a tutor for Python concepts and standard-library behavior;
- a guide to official GitLab and library documentation;
- a source of small experiments or counterexamples;
- a debugger for a clearly described failure;
- a reviewer of code already written by the maintainer.

## Assistance ladder

When stuck, use the least revealing level that can unblock the work:

1. Restate the expected and actual behavior.
2. Reduce the problem to a small failing example or test.
3. Read the relevant traceback and primary documentation.
4. Ask for an explanation of the concept or API without project code.
5. Ask for a hint about the next diagnostic step.
6. Ask for pseudocode or a partial outline.
7. Ask for review of the maintainer's current attempt.
8. Request a minimal targeted code fragment only after the earlier levels are insufficient.

A complete generated implementation is an exception and requires an explicit request. Even then, it should be studied, rewritten where necessary, and covered by tests the maintainer understands.

## A productive work session

A normal session should fit one small issue or one observable slice of it:

```text
read issue
  → write down expected behavior
  → create a failing example
  → implement
  → run and debug
  → review the diff
  → note what was learned
```

Stop at a working boundary. Do not compensate for a difficult task by quietly expanding scope or adding abstractions.

## Definition of learning done

An issue is complete only when the maintainer can explain:

- what inputs are accepted and rejected;
- why the chosen representation fits the current behavior;
- which edge cases the tests protect;
- what failure mode was hardest to diagnose;
- what would need to change for the next roadmap feature.

Perfect recall is not required. Copying code that cannot be explained is not completion.

## Review questions

Before opening a pull request, answer briefly:

- What did I decide myself?
- What did AI help with?
- Which test gives me the most confidence?
- What still feels unclear?

These are prompts for reflection, not bureaucracy. A few honest sentences are enough.
