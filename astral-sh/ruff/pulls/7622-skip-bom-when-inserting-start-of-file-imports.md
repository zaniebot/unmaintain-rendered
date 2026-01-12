```yaml
number: 7622
title: Skip BOM when inserting start-of-file imports
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/BOM
created_at: 2023-09-23T19:26:23Z
updated_at: 2023-09-23T19:43:00Z
url: https://github.com/astral-sh/ruff/pull/7622
synced_at: 2026-01-12T02:39:10Z
```

# Skip BOM when inserting start-of-file imports

---

_Pull request opened by @charliermarsh on 2023-09-23 19:26_

See: https://github.com/astral-sh/ruff/issues/7455#issuecomment-1732387485.

---

_Comment by @codspeed-hq[bot] on 2023-09-23 19:36_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/BOM)

### Merging #7622 will **degrade performances by 2.27%**

<sub>Comparing <code>charlie/BOM</code> (a1777d8) with <code>main</code> (b194f59)</sub>



### Summary

`❌ 1` regressions
`✅ 24` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/BOM)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/BOM` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[large/dataset.py]` | 92 ms | 94.1 ms | -2.27% |


---

_Merged by @charliermarsh on 2023-09-23 19:36_

---

_Closed by @charliermarsh on 2023-09-23 19:36_

---

_Branch deleted on 2023-09-23 19:36_

---

_Comment by @github-actions[bot] on 2023-09-23 19:42_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
