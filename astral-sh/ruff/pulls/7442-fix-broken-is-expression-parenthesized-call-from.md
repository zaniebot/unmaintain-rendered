```yaml
number: 7442
title: "Fix broken `is_expression_parenthesized` call from rebase"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/arg
created_at: 2023-09-16T17:13:40Z
updated_at: 2023-09-16T17:24:40Z
url: https://github.com/astral-sh/ruff/pull/7442
synced_at: 2026-01-12T15:55:23Z
```

# Fix broken `is_expression_parenthesized` call from rebase

---

_@charliermarsh_

_No description provided._

---

_@MichaReiser approved on 2023-09-16 17:17_

Lol, you too

---

_Label `internal` added by @MichaReiser on 2023-09-16 17:19_

---

_Merged by @MichaReiser on 2023-09-16 17:22_

---

_Closed by @MichaReiser on 2023-09-16 17:22_

---

_Branch deleted on 2023-09-16 17:22_

---

_Comment by @codspeed-hq[bot] on 2023-09-16 17:24_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/arg)

### Merging #7442 will **degrade performances by 4.57%**

<sub>:warning: No base runs were found</sub>

<sub>Falling back to comparing <code>charlie/arg</code> (74535ad) with <code>main</code> (1880cce)</sub>



### Summary

`❌ 1` regressions
`✅ 24` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/arg)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/arg` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `formatter[large/dataset.py]` | 58.9 ms | 61.7 ms | -4.57% |


---
