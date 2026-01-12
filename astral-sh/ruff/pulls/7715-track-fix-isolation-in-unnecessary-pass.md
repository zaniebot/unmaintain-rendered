```yaml
number: 7715
title: "Track fix isolation in `unnecessary-pass`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pass
created_at: 2023-09-29T17:05:13Z
updated_at: 2023-09-29T17:31:55Z
url: https://github.com/astral-sh/ruff/pull/7715
synced_at: 2026-01-12T15:55:24Z
```

# Track fix isolation in `unnecessary-pass`

---

_@charliermarsh_

## Summary

This wasn't necessary in the past, since we _only_ applied this rule to bodies that contained two statements, one of which was a `pass`. Now that it applies to any `pass` in a block with multiple statements, we can run into situations in which we remove both passes, and so need to apply the fixes in isolation.

See: https://github.com/astral-sh/ruff/issues/7455#issuecomment-1741107573.


---

_Label `bug` added by @charliermarsh on 2023-09-29 17:05_

---

_Comment by @codspeed-hq[bot] on 2023-09-29 17:18_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/pass)

### Merging #7715 will **improve performances by 4.05%**

<sub>Comparing <code>charlie/pass</code> (8c2c7cf) with <code>main</code> (974262a)</sub>



### Summary

`⚡ 1` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/pass` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[large/dataset.py]` | 95.7 ms | 92 ms | +4.05% |


---

_Merged by @charliermarsh on 2023-09-29 17:23_

---

_Closed by @charliermarsh on 2023-09-29 17:23_

---

_Branch deleted on 2023-09-29 17:23_

---

_Comment by @github-actions[bot] on 2023-09-29 17:31_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
