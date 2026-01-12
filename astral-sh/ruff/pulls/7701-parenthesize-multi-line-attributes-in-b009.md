```yaml
number: 7701
title: Parenthesize multi-line attributes in B009
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - fuzzer
assignees: []
merged: true
base: main
head: charlie/B009
created_at: 2023-09-28T20:42:14Z
updated_at: 2023-09-28T21:04:47Z
url: https://github.com/astral-sh/ruff/pull/7701
synced_at: 2026-01-12T02:39:10Z
```

# Parenthesize multi-line attributes in B009

---

_Pull request opened by @charliermarsh on 2023-09-28 20:42_

See https://github.com/astral-sh/ruff/issues/7455#issuecomment-1739800901.


---

_Label `bug` added by @charliermarsh on 2023-09-28 20:42_

---

_Label `fuzzer` added by @charliermarsh on 2023-09-28 20:42_

---

_Comment by @codspeed-hq[bot] on 2023-09-28 20:50_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/B009)

### Merging #7701 will **degrade performances by 2.81%**

<sub>Comparing <code>charlie/B009</code> (c3951ef) with <code>main</code> (f452813)</sub>



### Summary

`❌ 1` regressions
`✅ 24` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/B009)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/B009` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[numpy/ctypeslib.py]` | 18.1 ms | 18.6 ms | -2.81% |


---

_Merged by @charliermarsh on 2023-09-28 20:59_

---

_Closed by @charliermarsh on 2023-09-28 20:59_

---

_Branch deleted on 2023-09-28 20:59_

---

_Comment by @github-actions[bot] on 2023-09-28 20:59_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @qarmin on 2023-09-28 21:04_

PR closed entire issue

---

_Comment by @charliermarsh on 2023-09-28 21:04_

Thx.

---
