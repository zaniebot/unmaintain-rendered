```yaml
number: 22721
title: Skip EXE0xx tests on WSL
type: pull_request
state: merged
author: K900
labels:
  - testing
assignees: []
merged: true
base: main
head: wsl-tests
created_at: 2026-01-19T13:26:09Z
updated_at: 2026-01-19T13:31:55Z
url: https://github.com/astral-sh/ruff/pull/22721
synced_at: 2026-01-19T14:36:03Z
```

# Skip EXE0xx tests on WSL

---

_@K900_

The rule itself is always ignored on WSL, which makes the tests fail due to false negatives when run in a WSL environment.

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Label `testing` added by @MichaReiser on 2026-01-19 13:27_

---

_@MichaReiser approved on 2026-01-19 13:27_

Thank you

---

_Merged by @MichaReiser on 2026-01-19 13:31_

---

_Closed by @MichaReiser on 2026-01-19 13:31_

---
