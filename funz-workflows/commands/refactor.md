---
description: Propose and apply safe refactors to improve Python code quality
argument-hint: "<file, function, or path to refactor>"
allowed-tools:
  - Bash
  - Read
  - Edit
  - Grep
  - Glob
---

# Refactor

Refactor the target: **$ARGUMENTS**

Goal: improve readability, structure, and maintainability **without changing
observable behavior**.

## Steps

1. Read the target and its call sites (`grep` for usages) so you understand the
   contract before changing anything.
2. Identify concrete refactoring opportunities:
   - Extract long functions into well-named smaller ones.
   - Remove duplication; introduce helpers or dataclasses where they clarify.
   - Replace unclear names; add or tighten type hints.
   - Simplify nested conditionals (early returns, guard clauses).
   - Replace manual loops with comprehensions/`enumerate`/`zip` where it reads
     better — not where it hurts clarity.
   - Use context managers, `pathlib`, and stdlib idioms over ad-hoc code.
3. Confirm a safety net exists. Locate the relevant tests. If none cover the
   target, say so and recommend adding characterization tests before refactoring;
   proceed only if the user accepts the risk.
4. Apply changes in small, reviewable edits. Preserve public signatures unless the
   user asked otherwise.
5. Verify: run the test suite (`pytest`) and any linter/formatter the repo uses
   (`ruff`, `black`, `mypy`). Report results.

## Output

A short summary of what changed and why, the verification results, and any
follow-up refactors you deliberately left out of scope.
