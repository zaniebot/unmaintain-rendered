```yaml
number: 7562
title: "Detect `asyncio.get_running_loop` calls in RUF006"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/create_task
created_at: 2023-09-21T04:25:43Z
updated_at: 2023-09-21T04:43:50Z
url: https://github.com/astral-sh/ruff/pull/7562
synced_at: 2026-01-12T02:39:10Z
```

# Detect `asyncio.get_running_loop` calls in RUF006

---

_Pull request opened by @charliermarsh on 2023-09-21 04:25_

## Summary

We can do a good enough job detecting this with our existing semantic model.

Closes https://github.com/astral-sh/ruff/issues/3237.


---

_Label `rule` added by @charliermarsh on 2023-09-21 04:25_

---

_Comment by @codspeed-hq[bot] on 2023-09-21 04:36_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/create_task)

### Merging #7562 will **improve performances by 3.25%**

<sub>Comparing <code>charlie/create_task</code> (35bbae6) with <code>main</code> (b685ee4)</sub>



### Summary

`⚡ 1` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/create_task` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[large/dataset.py]` | 94.1 ms | 91.2 ms | +3.25% |


---

_Merged by @charliermarsh on 2023-09-21 04:37_

---

_Closed by @charliermarsh on 2023-09-21 04:37_

---

_Branch deleted on 2023-09-21 04:37_

---

_Comment by @github-actions[bot] on 2023-09-21 04:43_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
