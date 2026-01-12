```yaml
number: 7461
title: Add padding to prevent some autofix errors
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pad
created_at: 2023-09-17T15:05:07Z
updated_at: 2023-09-17T15:20:46Z
url: https://github.com/astral-sh/ruff/pull/7461
synced_at: 2026-01-12T02:39:10Z
```

# Add padding to prevent some autofix errors

---

_Pull request opened by @charliermarsh on 2023-09-17 15:05_

## Summary

We should really be doing this anywhere we _replace_ an expression.

See: https://github.com/astral-sh/ruff/issues/7455.

---

_Label `bug` added by @charliermarsh on 2023-09-17 15:05_

---

_Comment by @codspeed-hq[bot] on 2023-09-17 15:14_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/pad)

### Merging #7461 will **degrade performances by 2.2%**

<sub>Comparing <code>charlie/pad</code> (6c37745) with <code>main</code> (26ae0a6)</sub>



### Summary

`❌ 1` regressions
`✅ 24` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/pad)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/pad` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[pydantic/types.py]` | 72.4 ms | 74 ms | -2.2% |


---

_Merged by @charliermarsh on 2023-09-17 15:14_

---

_Closed by @charliermarsh on 2023-09-17 15:14_

---

_Branch deleted on 2023-09-17 15:14_

---

_Comment by @github-actions[bot] on 2023-09-17 15:20_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
