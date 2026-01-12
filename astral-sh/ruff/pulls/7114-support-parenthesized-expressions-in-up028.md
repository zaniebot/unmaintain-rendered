```yaml
number: 7114
title: Support parenthesized expressions in UP028
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - fuzzer
assignees: []
merged: true
base: main
head: charlie/UP028
created_at: 2023-09-03T21:10:10Z
updated_at: 2023-09-03T21:32:53Z
url: https://github.com/astral-sh/ruff/pull/7114
synced_at: 2026-01-12T15:55:23Z
```

# Support parenthesized expressions in UP028

---

_@charliermarsh_

Closes https://github.com/astral-sh/ruff/issues/7103.

---

_Label `bug` added by @charliermarsh on 2023-09-03 21:10_

---

_Label `fuzzer` added by @charliermarsh on 2023-09-03 21:10_

---

_Comment by @codspeed-hq[bot] on 2023-09-03 21:20_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/UP028)

### Merging #7114 will **degrade performances by 2.32%**

<sub>Comparing <code>charlie/UP028</code> (8216003) with <code>main</code> (911d4f2)</sub>



### Summary

`❌ 1` regressions
`✅ 19` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/UP028)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/UP028` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[numpy/ctypeslib.py]` | 33 ms | 33.8 ms | -2.32% |


---

_Merged by @charliermarsh on 2023-09-03 21:20_

---

_Closed by @charliermarsh on 2023-09-03 21:20_

---

_Branch deleted on 2023-09-03 21:20_

---

_Comment by @github-actions[bot] on 2023-09-03 21:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
