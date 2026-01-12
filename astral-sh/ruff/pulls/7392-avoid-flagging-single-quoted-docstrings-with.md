```yaml
number: 7392
title: Avoid flagging single-quoted docstrings with continuations for multi-line rules
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - docstring
assignees: []
merged: true
base: main
head: charlie/triple
created_at: 2023-09-14T18:48:03Z
updated_at: 2023-09-14T19:07:06Z
url: https://github.com/astral-sh/ruff/pull/7392
synced_at: 2026-01-12T02:39:10Z
```

# Avoid flagging single-quoted docstrings with continuations for multi-line rules

---

_Pull request opened by @charliermarsh on 2023-09-14 18:48_

Rules like D209 and D205 are only intended to apply to multi-line docstrings. If a docstring is single-quoted, but extends via a continuation, it should be excluded (it'll be flagged by another rule anyway). Closes https://github.com/astral-sh/ruff/issues/7058.

---

_Label `bug` added by @charliermarsh on 2023-09-14 18:50_

---

_Label `docstring` added by @charliermarsh on 2023-09-14 18:50_

---

_Merged by @charliermarsh on 2023-09-14 18:58_

---

_Closed by @charliermarsh on 2023-09-14 18:58_

---

_Branch deleted on 2023-09-14 18:58_

---

_Comment by @codspeed-hq[bot] on 2023-09-14 18:58_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/triple)

### Merging #7392 will **improve performances by 2.25%**

<sub>Comparing <code>charlie/triple</code> (fcf8d80) with <code>main</code> (f9e3ea2)</sub>



### Summary

`⚡ 1` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/triple` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[numpy/ctypeslib.py]` | 34.8 ms | 34.1 ms | +2.25% |


---

_Comment by @github-actions[bot] on 2023-09-14 19:07_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
