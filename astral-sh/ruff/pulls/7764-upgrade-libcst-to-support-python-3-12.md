```yaml
number: 7764
title: Upgrade LibCST to support Python 3.12
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/libcst
created_at: 2023-10-02T14:57:08Z
updated_at: 2023-10-02T16:14:36Z
url: https://github.com/astral-sh/ruff/pull/7764
synced_at: 2026-01-12T15:55:24Z
```

# Upgrade LibCST to support Python 3.12

---

_@charliermarsh_

## Summary

We'll revert back to the crates.io release once it's up-to-date, but better to get this out now that Python 3.12 is released. 

## Test Plan

`cargo test`


---

_Review requested from @zanieb by @charliermarsh on 2023-10-02 14:58_

---

_Review requested from @dhruvmanila by @charliermarsh on 2023-10-02 14:58_

---

_Label `internal` added by @charliermarsh on 2023-10-02 15:08_

---

_Comment by @codspeed-hq[bot] on 2023-10-02 15:13_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/libcst)

### Merging #7764 will **degrade performances by 2.09%**

<sub>Comparing <code>charlie/libcst</code> (e7324d9) with <code>main</code> (bdf2852)</sub>



### Summary

`❌ 1` regressions
`✅ 24` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/libcst)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/libcst` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[pydantic/types.py]` | 38.1 ms | 38.9 ms | -2.09% |


---

_Review requested from @konstin by @charliermarsh on 2023-10-02 15:26_

---

_Comment by @github-actions[bot] on 2023-10-02 15:26_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@konstin approved on 2023-10-02 15:28_

---

_Merged by @charliermarsh on 2023-10-02 16:14_

---

_Closed by @charliermarsh on 2023-10-02 16:14_

---

_Branch deleted on 2023-10-02 16:14_

---
