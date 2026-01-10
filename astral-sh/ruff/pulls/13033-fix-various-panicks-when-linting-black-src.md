```yaml
number: 13033
title: Fix various panicks when linting black/src
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: fix-panics-when-checking-black
created_at: 2024-08-21T12:30:40Z
updated_at: 2024-08-21T12:45:11Z
url: https://github.com/astral-sh/ruff/pull/13033
synced_at: 2026-01-10T21:38:32Z
```

# Fix various panicks when linting black/src

---

_Pull request opened by @MichaReiser on 2024-08-21 12:30_

## Summary

Fixes various panics when checking `black/src` due to TODO comments/not visited expressions

## Test Plan

I ran red knot over `black/src`


---

_Review requested from @carljm by @MichaReiser on 2024-08-21 12:30_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-21 12:30_

---

_Label `red-knot` added by @MichaReiser on 2024-08-21 12:30_

---

_@AlexWaygood approved on 2024-08-21 12:31_

---

_Merged by @MichaReiser on 2024-08-21 12:35_

---

_Closed by @MichaReiser on 2024-08-21 12:35_

---

_Branch deleted on 2024-08-21 12:35_

---

_Comment by @codspeed-hq[bot] on 2024-08-21 12:36_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/fix-panics-when-checking-black)

### Merging #13033 will **improve performances by 7.97%**

<sub>Comparing <code>fix-panics-when-checking-black</code> (ec58096) with <code>main</code> (0c98b59)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `fix-panics-when-checking-black` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[numpy/globals.py]` | 184.1 µs | 170.5 µs | +7.97% |


---

_Comment by @github-actions[bot] on 2024-08-21 12:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
