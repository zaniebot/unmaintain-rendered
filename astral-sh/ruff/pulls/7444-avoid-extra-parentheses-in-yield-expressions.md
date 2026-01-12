```yaml
number: 7444
title: "Avoid extra parentheses in `yield` expressions"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/yield
created_at: 2023-09-16T17:39:57Z
updated_at: 2023-09-16T18:46:57Z
url: https://github.com/astral-sh/ruff/pull/7444
synced_at: 2026-01-12T02:39:10Z
```

# Avoid extra parentheses in `yield` expressions

---

_Pull request opened by @charliermarsh on 2023-09-16 17:39_

## Summary

This is equivalent to https://github.com/astral-sh/ruff/pull/7424, but for `yield` and `yield from` expressions. Specifically, we want to avoid adding unnecessary extra parentheses for `yield expr` when `expr` itself does not require parentheses.

## Test Plan

`cargo test`

No change in any of the similarity metrics.

Before:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1632 |
| django       |           0.99982 |              2760 |                37 |
| transformers |           0.99957 |              2587 |               399 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99983 |              3496 |                18 |
| warehouse    |           0.99929 |               648 |                16 |
| zulip        |           0.99962 |              1437 |                22 |

After:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1632 |
| django       |           0.99982 |              2760 |                37 |
| transformers |           0.99957 |              2587 |               399 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99983 |              3496 |                18 |
| warehouse    |           0.99929 |               648 |                16 |
| zulip        |           0.99962 |              1437 |                22 |


---

_Label `formatter` added by @charliermarsh on 2023-09-16 17:40_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-16 17:44_

---

_Comment by @codspeed-hq[bot] on 2023-09-16 17:49_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/yield)

### Merging #7444 will **improve performances by 4.76%**

<sub>Comparing <code>charlie/yield</code> (ed6d3cb) with <code>main</code> (aae02cf)</sub>



### Summary

`⚡ 1` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/yield` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `formatter[large/dataset.py]` | 61.7 ms | 58.9 ms | +4.76% |


---

_@MichaReiser approved on 2023-09-16 18:32_

---

_Merged by @charliermarsh on 2023-09-16 18:46_

---

_Closed by @charliermarsh on 2023-09-16 18:46_

---

_Branch deleted on 2023-09-16 18:46_

---
