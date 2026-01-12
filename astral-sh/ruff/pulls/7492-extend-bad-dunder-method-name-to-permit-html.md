```yaml
number: 7492
title: "Extend `bad-dunder-method-name` to permit `__html__`"
type: pull_request
state: merged
author: jaap3
labels: []
assignees: []
merged: true
base: main
head: allow-dunder-html
created_at: 2023-09-18T15:04:37Z
updated_at: 2023-09-18T16:47:06Z
url: https://github.com/astral-sh/ruff/pull/7492
synced_at: 2026-01-12T15:55:24Z
```

# Extend `bad-dunder-method-name` to permit `__html__`

---

_@jaap3_

## Summary

Fixes #7478

## Test Plan

`cargo test` 

---

_@dhruvmanila approved on 2023-09-18 15:07_

Thanks!

---

_Comment by @codspeed-hq[bot] on 2023-09-18 15:13_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/jaap3:allow-dunder-html)

### Merging #7492 will **improve performances by 4.77%**

<sub>Comparing <code>jaap3:allow-dunder-html</code> (83a1e9b) with <code>main</code> (8ab2519)</sub>



### Summary

`⚡ 1` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `jaap3:allow-dunder-html` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `formatter[large/dataset.py]` | 61.7 ms | 58.9 ms | +4.77% |


---

_Merged by @dhruvmanila on 2023-09-18 15:16_

---

_Closed by @dhruvmanila on 2023-09-18 15:16_

---

_Comment by @github-actions[bot] on 2023-09-18 15:22_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Branch deleted on 2023-09-18 16:47_

---
