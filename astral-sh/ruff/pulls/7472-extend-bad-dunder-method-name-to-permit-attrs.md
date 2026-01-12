```yaml
number: 7472
title: "Extend `bad-dunder-method-name` to permit `attrs` dunders"
type: pull_request
state: merged
author: tjkuson
labels:
  - bug
assignees: []
merged: true
base: main
head: extend-bad-dunder-method-name
created_at: 2023-09-17T19:49:50Z
updated_at: 2023-09-18T09:04:28Z
url: https://github.com/astral-sh/ruff/pull/7472
synced_at: 2026-01-12T15:55:24Z
```

# Extend `bad-dunder-method-name` to permit `attrs` dunders

---

_@tjkuson_

## Summary

Closes #7451.

## Test Plan

`cargo test`


---

_Comment by @codspeed-hq[bot] on 2023-09-17 19:58_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/tjkuson:extend-bad-dunder-method-name)

### Merging #7472 will **improve performances by 4.78%**

<sub>Comparing <code>tjkuson:extend-bad-dunder-method-name</code> (f8c7c4b) with <code>main</code> (28273eb)</sub>



### Summary

`⚡ 1` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `tjkuson:extend-bad-dunder-method-name` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `formatter[large/dataset.py]` | 61.8 ms | 58.9 ms | +4.78% |


---

_Comment by @github-actions[bot] on 2023-09-17 20:06_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Label `bug` added by @charliermarsh on 2023-09-17 20:13_

---

_Comment by @charliermarsh on 2023-09-17 20:13_

Thanks!

---

_Merged by @charliermarsh on 2023-09-17 20:13_

---

_Closed by @charliermarsh on 2023-09-17 20:13_

---

_Branch deleted on 2023-09-18 09:04_

---
