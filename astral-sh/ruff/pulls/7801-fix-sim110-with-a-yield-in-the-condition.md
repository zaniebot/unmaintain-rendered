```yaml
number: 7801
title: Fix SIM110 with a yield in the condition
type: pull_request
state: merged
author: JelleZijlstra
labels:
  - bug
assignees: []
merged: true
base: main
head: sim110
created_at: 2023-10-04T05:13:56Z
updated_at: 2024-09-10T23:38:39Z
url: https://github.com/astral-sh/ruff/pull/7801
synced_at: 2026-01-10T21:38:31Z
```

# Fix SIM110 with a yield in the condition

---

_Pull request opened by @JelleZijlstra on 2023-10-04 05:13_

And allow "await" in the loop iterable.

Fixes #7800


---

_Comment by @codspeed-hq[bot] on 2023-10-04 05:27_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/JelleZijlstra:sim110)

### Merging #7801 will **degrade performances by 2.37%**

<sub>Comparing <code>JelleZijlstra:sim110</code> (e27ec42) with <code>main</code> (7b4fb4f)</sub>



### Summary

`⚡ 1` improvements
`❌ 1` regressions
`✅ 23` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/JelleZijlstra:sim110)._

### Benchmarks breakdown

|     | Benchmark | `main` | `JelleZijlstra:sim110` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[pydantic/types.py]` | 72.5 ms | 74.3 ms | -2.37% |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 38.9 ms | 38 ms | +2.48% |


---

_Comment by @github-actions[bot] on 2023-10-04 05:31_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@charliermarsh approved on 2023-10-04 12:59_

---

_Merged by @charliermarsh on 2023-10-04 12:59_

---

_Closed by @charliermarsh on 2023-10-04 12:59_

---

_Label `bug` added by @charliermarsh on 2023-10-04 12:59_

---

_Branch deleted on 2023-10-04 15:34_

---

_Branch restored on 2024-09-10 23:38_

---
