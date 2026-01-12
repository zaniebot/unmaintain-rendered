```yaml
number: 3700
title: Avoid panics for implicitly concatenated forward references
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/implicit-annotations
created_at: 2023-03-23T22:45:23Z
updated_at: 2023-03-23T23:20:00Z
url: https://github.com/astral-sh/ruff/pull/3700
synced_at: 2026-01-12T15:55:13Z
```

# Avoid panics for implicitly concatenated forward references

---

_@charliermarsh_

## Summary

Right now, annotations like `"""List[int]"""'☃'` can cause a panic, since we assume that the start and end quotes for a string are "aligned" -- which isn't the case for implicit concatenations. This PR makes the checks a bit more defensive.


---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/str.rs`:26 on 2023-03-23 22:46_

I wish this returned `None` for implicit concatenations.

---

_@charliermarsh reviewed on 2023-03-23 22:46_

---

_Comment by @github-actions[bot] on 2023-03-23 23:09_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.8±0.25ms     2.4 MB/sec    1.02     17.2±0.35ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.09ms     3.8 MB/sec    1.01      4.4±0.11ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   608.2±13.03µs     4.9 MB/sec    1.03   626.4±25.41µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.14ms     3.4 MB/sec    1.03      7.6±0.21ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.01      8.9±0.31ms     4.6 MB/sec    1.00      8.8±0.13ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1962.7±39.90µs     8.5 MB/sec    1.02  1998.3±88.83µs     8.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   240.3±10.54µs    12.3 MB/sec    1.01   241.7±14.79µs    12.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.2±0.11ms     6.1 MB/sec    1.03      4.3±0.34ms     6.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     12.7±0.07ms     3.2 MB/sec    1.00     12.7±0.08ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.06ms     5.1 MB/sec    1.00      3.3±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    372.5±1.65µs     7.9 MB/sec    1.00    372.0±2.00µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.5±0.03ms     4.7 MB/sec    1.00      5.5±0.09ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.02ms     5.8 MB/sec    1.01      7.1±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1470.4±4.96µs    11.3 MB/sec    1.01   1479.0±7.58µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    152.0±0.88µs    19.4 MB/sec    1.02    155.5±4.63µs    19.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.02ms     8.0 MB/sec    1.01      3.2±0.03ms     7.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-03-23 23:13_

---

_Closed by @charliermarsh on 2023-03-23 23:13_

---

_Branch deleted on 2023-03-23 23:13_

---
