# TCR Agent Skills

A collection of [Agent Skills](https://agentskills.io/specification) for **TCR (Test && Commit || Revert)** workflows.

---

## Skills

### `tcr` — TCR/TCRDD with git-gamble

Coaches users through **TCRDD** (TCR + TDD) using [`git-gamble`](https://git-gamble.is-cool.dev/).

- Explains the three phases (Red / Green / Refactor) and their `git gamble` flags
- Enforces strict TCR discipline: `git gamble` is the test run — no pre-running tests
- Coaches on surprise results and common mistakes

**Requirements:** `git`, [`git-gamble`](https://git-gamble.is-cool.dev/)

### `tcr-kentbeck` — Kent Beck's TCR

A faithful adaptation of [Kent Beck's own TCR skill](https://github.com/KentBeck/TCRSkill/), originally written for Cursor, made agent-agnostic per the [Agent Skills specification](https://agentskills.io/specification).

The agent makes tiny changes, runs the full test suite, commits on green, reverts on red — and logs every failure to `tcr-failure-log.md`. No tool dependencies beyond `git`; the agent detects and uses the project's existing test command.

**Requirements:** `git`

**Inspired by:** [KentBeck/TCRSkill](https://github.com/KentBeck/TCRSkill/)

---

## Installation

```bash
# Install tcr-kentbeck
npx skills install xpepper/tcr-skill --skill tcr-kentbeck

# Install tcr
npx skills install xpepper/tcr-skill --skill tcr
```

Alternatively, use the pre-packaged `.skill` files:

```bash
# Available at repo root: tcr.skill, tcr-kentbeck.skill
```

Refer to your agent's documentation for the exact skills directory.

---

## Evals

The `evals/` directory contains test prompts used to validate the skills.

---

## Resources

- [TCR original post by Kent Beck](https://medium.com/@kentbeck_7670/test-commit-revert-870bbd756864)
- [KentBeck/TCRSkill](https://github.com/KentBeck/TCRSkill/) — Kent Beck's original Cursor skill
- [git-gamble](https://git-gamble.is-cool.dev/) — the tool that powers the `tcr` skill
- [Agent Skills specification](https://agentskills.io/specification)
