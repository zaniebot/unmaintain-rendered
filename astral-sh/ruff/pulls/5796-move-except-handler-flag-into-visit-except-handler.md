```yaml
number: 5796
title: "Move except-handler flag into `visit_except_handler`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/except-handler
created_at: 2023-07-16T04:20:36Z
updated_at: 2023-07-16T04:52:00Z
url: https://github.com/astral-sh/ruff/pull/5796
synced_at: 2026-01-12T03:30:21Z
```

# Move except-handler flag into `visit_except_handler`

---

_Pull request opened by @charliermarsh on 2023-07-16 04:20_

## Summary

This is more similar to how these flags work in other contexts (e.g., `visit_annotation`), and also ensures that we unset it prior to visit the `orelse` and `finalbody` (a subtle bug).

---

_Renamed from "Move except-handler flag into visit_except_handler" to "Move except-handler flag into `visit_except_handler`" by @charliermarsh on 2023-07-16 04:20_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:1955 on 2023-07-16 04:21_

Also moving these up to the "pre-visit" phase with all the other rules -- no reason for them to be here, in the "recurse" phase.

---

_@charliermarsh reviewed on 2023-07-16 04:21_

---

_Label `internal` added by @charliermarsh on 2023-07-16 04:21_

---

_Comment by @github-actions[bot] on 2023-07-16 04:31_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.02ms     4.4 MB/sec    1.01      9.4±0.02ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1862.2±2.01µs     8.9 MB/sec    1.00   1868.3±3.18µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    209.3±0.44µs    14.1 MB/sec    1.00    210.2±0.51µs    14.0 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.3 MB/sec    1.00      4.0±0.02ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.07     14.0±0.03ms     2.9 MB/sec    1.00     13.2±0.03ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      3.5±0.00ms     4.7 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.02    462.1±0.74µs     6.4 MB/sec    1.00    451.0±1.20µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.06      6.3±0.01ms     4.0 MB/sec    1.00      6.0±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.14      7.6±0.02ms     5.4 MB/sec    1.00      6.6±0.02ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.11  1623.3±12.79µs    10.3 MB/sec    1.00   1456.7±1.73µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.07    178.7±0.29µs    16.5 MB/sec    1.00    166.8±0.50µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.12      3.3±0.01ms     7.6 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.8±0.07ms     3.8 MB/sec    1.00     10.8±0.08ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.02ms     7.8 MB/sec    1.00      2.1±0.03ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    242.3±3.75µs    12.2 MB/sec    1.09   264.3±13.48µs    11.2 MB/sec
formatter/pydantic/types.py                1.00      4.6±0.04ms     5.5 MB/sec    1.01      4.7±0.05ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.09     18.1±0.42ms     2.2 MB/sec    1.00     16.6±1.16ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.23      5.0±0.25ms     3.3 MB/sec    1.00      4.1±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.13   563.4±34.85µs     5.2 MB/sec    1.00    500.0±7.61µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.22      8.5±0.42ms     3.0 MB/sec    1.00      6.9±0.05ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.19      9.5±0.30ms     4.3 MB/sec    1.00      7.9±0.11ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.13  1918.2±52.86µs     8.7 MB/sec    1.00  1692.1±16.79µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.01    203.0±4.25µs    14.5 MB/sec    1.00    201.5±6.63µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.13      4.0±0.17ms     6.4 MB/sec    1.00      3.5±0.03ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-16 04:35_

---

_Closed by @charliermarsh on 2023-07-16 04:35_

---

_Branch deleted on 2023-07-16 04:35_

---
