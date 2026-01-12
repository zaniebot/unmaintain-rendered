```yaml
number: 7497
title: "Ignore `pass-statement-stub-body` documentation formatting"
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-ci
created_at: 2023-09-18T17:49:04Z
updated_at: 2023-09-18T18:33:43Z
url: https://github.com/astral-sh/ruff/pull/7497
synced_at: 2026-01-12T02:39:10Z
```

# Ignore `pass-statement-stub-body` documentation formatting

---

_Pull request opened by @tjkuson on 2023-09-18 17:49_

## Summary

Fix CI (broken in #7496).

The code snippet was formatted as Black would format a stub file, but the CI script doesn't know that (it assumes all code snippets are non-stub files). Easier to ignore.

Sorry for breaking CI!

## Test Plan

`python scripts/check_docs_formatted.py`


---

_Comment by @codspeed-hq[bot] on 2023-09-18 17:57_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/tjkuson:fix-ci)

### Merging #7497 will **improve performances by 4.87%**

<sub>Comparing <code>tjkuson:fix-ci</code> (49a1d41) with <code>main</code> (bccba5d)</sub>



### Summary

`⚡ 1` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `tjkuson:fix-ci` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `formatter[large/dataset.py]` | 61.8 ms | 58.9 ms | +4.87% |


---

_Comment by @github-actions[bot] on 2023-09-18 18:06_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Merged by @charliermarsh on 2023-09-18 18:27_

---

_Closed by @charliermarsh on 2023-09-18 18:27_

---

_Label `documentation` added by @charliermarsh on 2023-09-18 18:27_

---

_Comment by @charliermarsh on 2023-09-18 18:27_

Np!

---

_Branch deleted on 2023-09-18 18:33_

---
