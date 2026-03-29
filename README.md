# tcr

An [Agent Skill](https://agentskills.io/specification) that guides users through **TCR/TCRDD** (TCR + TDD) using [`git-gamble`](https://git-gamble.is-cool.dev/).

## What it does

Coaches users through the TCR (Test && Commit || Revert) workflow layered on top of TDD:

- Explains the three TCR/TCRDD phases (Red / Green / Refactor) and their `git gamble` flags
- Enforces the strict TCR rule: `git gamble` is the test run — no pre-running tests
- Coaches when a surprise result (unexpected pass or fail) happens and why
- Guards against common mistakes (pre-checking tests, leaving dirty worktrees, writing all tests upfront)

## When it triggers

Triggers on: `TCR`, `TCRDD`, `test commit revert`, `git gamble`, `git-gamble`, or phrases like "develop with TDD and TCR" / "use TCR to force smallest steps".

**Does not** trigger for general TDD-only questions, unit testing setup, CI pipelines, or mocking — those don't need this skill.

## Requirements

- `git`
- [`git-gamble`](https://git-gamble.is-cool.dev/)

## Installation

Copy the `tcr/` directory into wherever your agent runtime loads skills from, e.g.:

```bash
cp -r tcr ~/.agents/skills/tcr
```

Refer to your agent's documentation for the exact skills directory and any symlinking needed.

## Evals

The `evals/` directory contains test cases used to validate the skill:

- `evals.json` — 3 functional test prompts (FizzBuzz TCR/TCRDD walkthrough, TCR mistake detection, TDD compliance review)
- `trigger_eval.json` — 20 trigger/no-trigger queries for description optimization

## Resources

- [git-gamble](https://git-gamble.is-cool.dev/) — the tool that implements TCR
- [TCR original post by Kent Beck](https://medium.com/@kentbeck_7670/test-commit-revert-870bbd756864)
- [Agent Skills specification](https://agentskills.io/specification)
