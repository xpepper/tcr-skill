---
name: tcr-kentbeck
description: >
  Enforce the TCR (test && commit || revert) discipline inspired by Kent Beck's original workflow.
  The agent makes tiny changes, runs the full test suite, and either commits on green or reverts
  everything on red — never accumulating broken code.
  ALWAYS trigger when the user mentions "TCR", "test commit revert", "Kent Beck TCR",
  "tcr-kentbeck", or asks to work with an automatic commit-or-revert loop.
  Trigger when the user wants to enforce baby-step discipline, avoid accumulating broken code,
  or practice micro-change development where every edit is immediately validated.
  Do NOT trigger for general TDD questions, test framework setup, CI pipelines,
  or code coverage — those do not need this skill.
license: MIT
compatibility: Requires git
---

# TCR — Test && Commit || Revert

TCR is a development discipline created by Kent Beck. The rule is simple:

> After every change, run the tests. If they pass, commit. If they fail, revert.

There is no middle ground. You never sit with broken code. You never "fix it in a minute."
The revert is immediate and non-negotiable.

This forces you to work in tiny steps — each one small enough that losing it to a revert
is cheap. Over time, this builds a codebase through a long sequence of provably-correct
micro-changes.

## The TCR Cycle

Every change follows this exact sequence:

```
1. Make a minimal change
2. Run the full test suite
3. Tests pass → stage and commit
4. Tests fail → revert all uncommitted changes
```

Then repeat.

### Step 1 — Make a Minimal Change

Make the smallest edit that moves toward your goal. A single function, a renamed variable,
one new test — no more. The smaller the change, the less painful a revert, and the more
precisely you learn what works and what doesn't.

If you're unsure whether a change is small enough, it probably isn't. Split it further.

### Step 2 — Run the Full Test Suite

Run the project's actual, complete test command. Not a filtered subset. Not a single file.
The full suite, every time.

Why? A filtered run might miss regressions in code you didn't think you touched. TCR's
power comes from the guarantee that the *entire* codebase is green after every commit.

#### Auto-detecting the test command

Look for the project's test command in this order:

1. **package.json** — check `scripts.test` (run with `npm test` or equivalent)
2. **Makefile / Taskfile** — look for a `test` target
3. **Standard runners** — `pytest`, `cargo test`, `go test ./...`, `dotnet test`, `mix test`, `bundle exec rspec`, `mvn test`, `gradle test`
4. **CI configuration** — `.github/workflows/`, `.circleci/`, `Jenkinsfile`, etc.
5. **Project documentation** — README or CONTRIBUTING files mentioning how to run tests

Use the first match. If none is found, ask the user.

### Step 3 — Green: Commit

When tests pass, immediately stage all changes and commit with a clear, descriptive message
that explains *what* the change does.

Do not batch multiple changes into one commit. Each TCR cycle produces exactly one commit.
This keeps the git history granular and bisectable.

### Step 4 — Red: Revert

When tests fail, revert **all** uncommitted changes immediately. Do not try to fix the
failing code — just revert. The working state is sacred.

```bash
git checkout -- .
git clean -fd
```

The revert is not punishment. It's information. The change you attempted didn't work in
its current form. Now you get to try a different, probably smaller, approach.

## The Failure Log

After every revert, append an entry to `tcr-failure-log.md` in the project root.
Create the file if it doesn't exist.

Each entry records what you tried and what went wrong, preserving learning without
cluttering the git history (since the reverted code is gone).

### Entry format

```markdown
## <timestamp>

**Change attempted:** <brief description of what you tried to do>

**Tests failed:**
- <test name or file>: <error summary>

**Why it failed:** <your best understanding of the root cause>

**Next approach:** <what you'll try differently>
```

### Example

```markdown
## 2025-06-15T14:32:00Z

**Change attempted:** Extract validation logic into a `validate_email` helper

**Tests failed:**
- test_user_registration: AttributeError — 'User' object has no attribute 'validate'

**Why it failed:** Renamed the method but forgot to update the call site in the User model

**Next approach:** Make the rename in a single step that covers both definition and all call sites
```

This log is valuable. It captures the dead ends so you don't repeat them, and it shows
the trajectory of your thinking.

## Exceptions

- **Documentation-only changes** (README, comments, markdown files) that have zero code
  impact may bypass the test cycle — just commit directly.
- The user can request a **temporary TCR suspension** for a specific task. Honor the
  request, but resume TCR discipline as soon as that task is done.

## Working with the Agent

When an AI agent is practicing TCR:

1. **Start from a clean worktree.** Before each cycle, ensure there are no unrelated
   uncommitted changes. TCR reverts the *entire* dirty worktree — unrelated edits will
   be lost in a revert.

2. **One change per cycle.** Do not combine multiple logical changes. If you need to
   rename a function and add a new test, those are two separate cycles.

3. **No pre-checking.** Do not run the test suite "to see where things stand" before
   making your change. Make the change, then test. The result should inform you, not
   confirm what you already know.

4. **Commit messages matter.** Each commit should describe the micro-change clearly.
   Since TCR produces many small commits, good messages make the history navigable.

5. **Revert without hesitation.** When tests fail, revert immediately. Do not attempt
   to "quickly fix" the issue. The revert restores the last known-good state. From
   there, try a different (usually smaller) approach.

6. **Log every revert.** Always append to `tcr-failure-log.md` after reverting. This
   is not optional — the log is how learning survives the revert.

7. **Detect the test command once.** At the start of the session, figure out how the
   project runs its tests. Use that command consistently throughout.

## The Core Principle

> Never accumulate uncommitted red work. Never commit on red.

Every commit in the history is green. Every failed attempt is reverted and logged.
The codebase moves forward through a chain of tiny, verified steps.
