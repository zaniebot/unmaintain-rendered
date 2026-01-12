```yaml
number: 7429
title: Avoiding grouping comprehension targets separately from conditions
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/comp
created_at: 2023-09-16T02:46:32Z
updated_at: 2023-09-16T17:19:35Z
url: https://github.com/astral-sh/ruff/pull/7429
synced_at: 2026-01-12T02:39:10Z
```

# Avoiding grouping comprehension targets separately from conditions

---

_Pull request opened by @charliermarsh on 2023-09-16 02:46_

## Summary

Black seems to treat comprehension targets and conditions as if they're in a single group -- so if the comprehension expands, all conditions are rendered on their own line, etc.

Closes https://github.com/astral-sh/ruff/issues/7421.

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
| warehouse    |           0.99923 |               648 |                18 |
| zulip        |           0.99962 |              1437 |                22 |

After:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1632 |
| django       |           0.99982 |              2760 |                37 |
| transformers |           0.99957 |              2587 |               399 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99983 |              3496 |                18 |
| warehouse    |           0.99923 |               648 |                18 |
| zulip        |           0.99962 |              1437 |                22 |


---

_Label `formatter` added by @charliermarsh on 2023-09-16 02:46_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-16 02:54_

---

_Review requested from @konstin by @charliermarsh on 2023-09-16 02:54_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/comprehension.rs`:59 on 2023-09-16 14:47_

The `format_args` here isn't necessary. You can pass all elements directly to `write`
```suggestion
```

Sorry, GitHub doesn't allow me to make the suggestion :(

---

_@MichaReiser approved on 2023-09-16 14:47_

---

_Comment by @codspeed-hq[bot] on 2023-09-16 17:18_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/comp)

### Merging #7429 will **degrade performances by 2.89%**

<sub>Comparing <code>charlie/comp</code> (f2f7185) with <code>main</code> (1880cce)</sub>



### Summary

`❌ 1` regressions
`✅ 24` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/comp)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/comp` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[numpy/ctypeslib.py]` | 34 ms | 35.1 ms | -2.89% |


---

_Merged by @charliermarsh on 2023-09-16 17:19_

---

_Closed by @charliermarsh on 2023-09-16 17:19_

---

_Branch deleted on 2023-09-16 17:19_

---
