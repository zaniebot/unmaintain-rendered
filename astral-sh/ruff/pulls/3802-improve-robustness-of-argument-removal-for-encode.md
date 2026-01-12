```yaml
number: 3802
title: "Improve robustness of argument removal for `encode` calls"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/up012
created_at: 2023-03-29T23:00:54Z
updated_at: 2023-03-29T23:24:33Z
url: https://github.com/astral-sh/ruff/pull/3802
synced_at: 2026-01-12T04:39:45Z
```

# Improve robustness of argument removal for `encode` calls

---

_Pull request opened by @charliermarsh on 2023-03-29 23:00_

## Summary

Migrates `UP012` to use our shared `remove_argument` helper, to replace its buggy argument-removal implementation. Also improves the diagnostic to use a separate suggestion for "Rewrite as a byte string" vs. "Remove the default argument" cases.

Closes #3797.


---

_Label `bug` added by @charliermarsh on 2023-03-29 23:00_

---

_Renamed from "Improve robustness of argument removal for encode calls" to "Improve robustness of argument removal for `encode` calls" by @charliermarsh on 2023-03-29 23:06_

---

_Merged by @charliermarsh on 2023-03-29 23:07_

---

_Closed by @charliermarsh on 2023-03-29 23:07_

---

_Branch deleted on 2023-03-29 23:07_

---

_Comment by @github-actions[bot] on 2023-03-29 23:15_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.3±0.01ms     2.8 MB/sec    1.00     14.4±0.02ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.00ms     4.5 MB/sec    1.00      3.7±0.01ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    411.0±2.16µs     7.2 MB/sec    1.00    409.8±2.47µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.01ms     4.1 MB/sec    1.00      6.3±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.01ms     5.3 MB/sec    1.00      7.7±0.01ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1699.6±7.81µs     9.8 MB/sec    1.01   1709.1±5.58µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.4±0.41µs    16.4 MB/sec    1.00    179.9±0.91µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.1 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.14     18.3±0.39ms     2.2 MB/sec    1.00     16.0±0.37ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.21      5.0±0.14ms     3.4 MB/sec    1.00      4.1±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.12   557.4±22.59µs     5.3 MB/sec    1.00    498.5±7.24µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.11      7.4±0.32ms     3.4 MB/sec    1.00      6.7±0.08ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.07      8.7±0.43ms     4.7 MB/sec    1.00      8.1±0.04ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1897.4±57.78µs     8.8 MB/sec    1.00  1819.0±30.34µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.03    202.0±6.18µs    14.6 MB/sec    1.00    195.6±3.98µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.07      4.0±0.10ms     6.4 MB/sec    1.00      3.8±0.04ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
