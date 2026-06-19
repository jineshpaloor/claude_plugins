---
description: Review a pull request for correctness, quality, and Python best practices
argument-hint: "<PR number or URL> (optional; defaults to current branch's PR)"
allowed-tools:
  - Bash
  - Read
  - Grep
  - Glob
---

# Review Pull Request

Review the pull request: **$ARGUMENTS**

If no PR is given, infer it from the current branch (`git branch --show-current`)
and the PR open against the default branch.

## Steps

1. Gather the diff. Use `gh pr diff $ARGUMENTS` if `gh` is available; otherwise
   `git diff <base>...HEAD`. Read the changed files in full when the diff alone
   is not enough to judge intent.
2. Build context. Identify the purpose of the change from the PR title/description
   and the surrounding code. Note the public surface that changed.
3. Review against these dimensions, in priority order:
   - **Correctness** — logic errors, edge cases, off-by-one, error handling,
     resource cleanup (context managers / `with`), concurrency issues.
   - **Python idioms** — PEP 8, type hints, comprehensions vs loops, dataclasses,
     avoiding mutable default args, proper exception types, f-strings.
   - **Tests** — is new behavior covered? Are tests meaningful (asserting outcomes,
     not implementation)? Flag missing coverage for changed branches.
   - **API & compatibility** — breaking changes to signatures, return types,
     or behavior; deprecations handled cleanly.
   - **Performance** — needless O(n^2), repeated DB/network calls in loops,
     large in-memory loads that could stream.
   - **Readability & maintainability** — naming, function size, duplication,
     dead code, unclear control flow.

## Output

Group findings by severity: **Blocking**, **Should fix**, **Nit**. For each, give
the file:line, a one-line problem statement, and a concrete suggested fix. End
with a short verdict (Approve / Approve with comments / Request changes) and a
two-sentence summary of the change's overall quality.
