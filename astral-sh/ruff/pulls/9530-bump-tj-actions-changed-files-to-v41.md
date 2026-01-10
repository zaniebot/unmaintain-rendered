```yaml
number: 9530
title: "Bump `tj-actions/changed-files` to v41"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/change
created_at: 2024-01-15T14:56:10Z
updated_at: 2024-01-15T15:34:44Z
url: https://github.com/astral-sh/ruff/pull/9530
synced_at: 2026-01-10T22:57:09Z
```

# Bump `tj-actions/changed-files` to v41

---

_Pull request opened by @charliermarsh on 2024-01-15 14:56_

This is a subset of https://github.com/astral-sh/ruff/pull/9526 that should be safe to apply.

---

_Review requested from @MichaReiser by @charliermarsh on 2024-01-15 14:56_

---

_Review requested from @zanieb by @charliermarsh on 2024-01-15 14:56_

---

_Label `internal` added by @charliermarsh on 2024-01-15 14:56_

---

_Comment by @codspeed-hq[bot] on 2024-01-15 15:03_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/change)

### Merging #9530 will **degrade performances by 4.35%**

<sub>Comparing <code>charlie/change</code> (65b455a) with <code>main</code> (0753968)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/change)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/change` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[numpy/ctypeslib.py]` | 12.2 ms | 12.8 ms | -4.35% |


---

_Comment by @github-actions[bot] on 2024-01-15 15:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2024-01-15 15:25_

Took me a while to find but the reason why they released a new major is because of 

> A new safe_output input is now available to prevent outputting unsafe filename characters (Enabled by default). This would escape characters in the filename that could be used for command injection.

Which shouldn't matter for us

---

_@MichaReiser approved on 2024-01-15 15:25_

---

_Merged by @charliermarsh on 2024-01-15 15:34_

---

_Closed by @charliermarsh on 2024-01-15 15:34_

---

_Branch deleted on 2024-01-15 15:34_

---
