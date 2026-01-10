```yaml
number: 9566
title: "[flake8-pyi] Fix PYI047 false negatives on PEP-695 type aliases"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
assignees: []
merged: true
base: main
head: y047-695
created_at: 2024-01-17T16:41:57Z
updated_at: 2024-01-18T07:16:50Z
url: https://github.com/astral-sh/ruff/pull/9566
synced_at: 2026-01-10T22:57:09Z
```

# [flake8-pyi] Fix PYI047 false negatives on PEP-695 type aliases

---

_Pull request opened by @AlexWaygood on 2024-01-17 16:41_

## Summary

Fixes one of the issues listed in https://github.com/astral-sh/ruff/issues/8771. Fairly straightforward!

## Test Plan

`cargo test` / `cargo insta review`


---

_Label `rule` added by @AlexWaygood on 2024-01-17 16:42_

---

_Comment by @codspeed-hq[bot] on 2024-01-17 16:48_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/AlexWaygood:y047-695)

### Merging #9566 will **degrade performances by 4.35%**

<sub>Comparing <code>AlexWaygood:y047-695</code> (209182b) with <code>main</code> (368e279)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/AlexWaygood:y047-695)._

### Benchmarks breakdown

|     | Benchmark | `main` | `AlexWaygood:y047-695` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[numpy/ctypeslib.py]` | 12.2 ms | 12.8 ms | -4.35% |


---

_Comment by @github-actions[bot] on 2024-01-17 16:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-01-18 03:13_

Nice, looking good!

---

_Merged by @charliermarsh on 2024-01-18 03:14_

---

_Closed by @charliermarsh on 2024-01-18 03:14_

---

_Branch deleted on 2024-01-18 07:16_

---
