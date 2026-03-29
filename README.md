# tcrdd — Claude Code Skill

A [Claude Code skill](https://docs.anthropic.com/en/docs/claude-code/skills) that guides users through **TCRDD** (TCR + TDD) using [`git-gamble`](https://git-gamble.is-cool.dev/).

## What it does

Coaches users through the TCR (Test && Commit || Revert) workflow layered on top of TDD:

- Explains the three TCRDD phases (Red / Green / Refactor) and their `git gamble` flags
- Enforces the strict TCR rule: `git gamble` is the test run — no pre-running tests
- Coaches when a surprise result (unexpected pass or fail) happens and why
- Guards against common mistakes (pre-checking tests, leaving dirty worktrees, writing all tests upfront)

## When it triggers

Triggers on: `TCR`, `TCRDD`, `test commit revert`, `git gamble`, `git-gamble`, or phrases like "develop with TDD and TCR" / "use TCR to force smallest steps".

**Does not** trigger for general TDD-only questions, unit testing setup, CI pipelines, or mocking — those don't need this skill. For pure TDD guidance, use the `superpowers:test-driven-development` skill instead.

## Installation

```bash
# Copy skill to your agents skills directory
cp -r tcrdd ~/.agents/skills/tcrdd

# Symlink into Claude Code's skills folder
ln -s ../../.agents/skills/tcrdd ~/.claude/skills/tcrdd
```

Or install the packaged `.skill` file if your setup supports it.

## Evals

The `evals/` directory contains test cases used to validate the skill:

- `evals.json` — 3 functional test prompts (FizzBuzz TCRDD walkthrough, TCR mistake detection, TDD compliance review)
- `trigger_eval.json` — 20 trigger/no-trigger queries for description optimization

Run evals with the [skill-creator](https://github.com/anthropics/claude-code) tooling:

```bash
cd ~/.agents/skills/skill-creator
python -m scripts.run_eval \
  --eval-set /path/to/evals/evals.json \
  --skill-path /path/to/tcrdd \
  --model claude-sonnet-4-6
```

## Resources

- [git-gamble](https://git-gamble.is-cool.dev/) — the tool that implements TCR
- [TCR original post by Kent Beck](https://medium.com/@kentbeck_7670/test-commit-revert-870bbd756864)
