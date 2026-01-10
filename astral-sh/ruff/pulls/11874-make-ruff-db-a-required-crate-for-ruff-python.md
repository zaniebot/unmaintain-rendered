```yaml
number: 11874
title: "Make `ruff_db` a required crate for `ruff_python_semantic`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/ruff-db
created_at: 2024-06-14T13:34:00Z
updated_at: 2024-06-14T13:48:16Z
url: https://github.com/astral-sh/ruff/pull/11874
synced_at: 2026-01-10T21:56:00Z
```

# Make `ruff_db` a required crate for `ruff_python_semantic`

---

_Pull request opened by @dhruvmanila on 2024-06-14 13:34_

## Summary

This PR makes the `ruff_db` a required crate for `ruff_python_semantic`.

Refer https://github.com/astral-sh/ruff/actions/runs/9516626143/job/26233307158?pr=11872

## Test Plan

1. `maturin sdist --out dist`
2. `tar -xf dist/ruff-0.4.8.tar.gz --directory=dist/ruff-0.4.8`
3. `pip install dist/ruff-0.4.8.tar.gz` works


---

_Label `internal` added by @dhruvmanila on 2024-06-14 13:34_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-14 13:34_

---

_Comment by @codspeed-hq[bot] on 2024-06-14 13:39_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/ruff-db)

### Merging #11874 will **degrade performances by 11.77%**

<sub>Comparing <code>dhruv/ruff-db</code> (995d1d3) with <code>main</code> (89bb07c)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/ruff-db)._

### Benchmarks breakdown

|     | Benchmark | `main` | `dhruv/ruff-db` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-with-preview-rules[numpy/globals.py]` | 803.8 µs | 911 µs | -11.77% |


---

_@MichaReiser approved on 2024-06-14 13:42_

---

_Merged by @MichaReiser on 2024-06-14 13:43_

---

_Closed by @MichaReiser on 2024-06-14 13:43_

---

_Branch deleted on 2024-06-14 13:43_

---

_Comment by @github-actions[bot] on 2024-06-14 13:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
