```yaml
number: 3739
title: "Use `wild::args()` and add `wild` as a dependency"
type: pull_request
state: merged
author: agriyakhetarpal
labels:
  - cli
assignees: []
merged: true
base: main
head: 3301-fix-windows-wildcards
created_at: 2023-03-26T10:19:27Z
updated_at: 2023-03-26T14:51:19Z
url: https://github.com/astral-sh/ruff/pull/3739
synced_at: 2026-01-12T15:55:13Z
```

# Use `wild::args()` and add `wild` as a dependency

---

_@agriyakhetarpal_

A small fix to close #3301 

### Summary
This replaces `std::env::args()` with `wild::args()` from https://gitlab.com/kornelski/wild and adds `wild` as a dependency in `ruff_cli/Cargo.toml`. 

### Checks

 * [x] Auto-formatting and linting
 * [x] Tests passing on local

---

_Comment by @github-actions[bot] on 2023-03-26 10:37_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     13.8±0.10ms     2.9 MB/sec    1.00     13.7±0.04ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.03ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    497.9±1.75µs     5.9 MB/sec    1.01    500.5±0.98µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.0±0.03ms     4.2 MB/sec    1.00      6.0±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.01      7.3±0.02ms     5.6 MB/sec    1.00      7.3±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1649.0±3.25µs    10.1 MB/sec    1.00   1645.0±1.24µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    187.7±1.13µs    15.7 MB/sec    1.01    188.7±0.67µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.00ms     7.5 MB/sec    1.00      3.4±0.01ms     7.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.4±0.09ms     2.6 MB/sec    1.00     15.4±0.13ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.02ms     4.0 MB/sec    1.00      4.1±0.01ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    457.1±4.25µs     6.5 MB/sec    1.00    457.4±4.55µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.9±0.07ms     3.7 MB/sec    1.00      6.8±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.02      8.4±0.04ms     4.9 MB/sec    1.00      8.3±0.06ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1817.2±12.69µs     9.2 MB/sec    1.00  1767.6±11.36µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.01    186.2±2.61µs    15.8 MB/sec    1.00    185.2±5.34µs    15.9 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.9±0.04ms     6.5 MB/sec    1.00      3.8±0.02ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-03-26 14:27_

Thank you!

---

_Renamed from "Use wild::args() and add wild as a dependency" to "Use `wild::args()` and add `wild` as a dependency" by @charliermarsh on 2023-03-26 14:28_

---

_Label `cli` added by @charliermarsh on 2023-03-26 14:31_

---

_Merged by @charliermarsh on 2023-03-26 14:32_

---

_Closed by @charliermarsh on 2023-03-26 14:32_

---

_Branch deleted on 2023-03-26 14:33_

---
