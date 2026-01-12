```yaml
number: 19109
title: "[`flake8-simplify`] Make example error out-of-the-box (`SIM113`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-6
created_at: 2025-07-02T23:52:50Z
updated_at: 2025-07-03T14:13:49Z
url: https://github.com/astral-sh/ruff/pull/19109
synced_at: 2026-01-12T15:56:32Z
```

# [`flake8-simplify`] Make example error out-of-the-box (`SIM113`)

---

_@MeGaGiGaGon_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Part of #18972

This PR makes [enumerate-for-loop (SIM113)](https://docs.astral.sh/ruff/rules/enumerate-for-loop/#enumerate-for-loop-sim113)'s example error out-of-the-box

[Old example](https://play.ruff.rs/a6ef6fec-eb6b-477c-a962-616f0b8e1491)
```py
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(f"{i + 1}. {fruit}")
    i += 1
```

[New example](https://play.ruff.rs/1811d608-1aa0-45d8-96dc-18105e74b8cc)
```py
fruits = ["apple", "banana", "cherry"]
i = 0
for fruit in fruits:
    print(f"{i + 1}. {fruit}")
    i += 1
```

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-07-03 00:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre approved on 2025-07-03 14:08_

---

_Label `documentation` added by @ntBre on 2025-07-03 14:08_

---

_Merged by @ntBre on 2025-07-03 14:08_

---

_Closed by @ntBre on 2025-07-03 14:08_

---

_Branch deleted on 2025-07-03 14:13_

---
