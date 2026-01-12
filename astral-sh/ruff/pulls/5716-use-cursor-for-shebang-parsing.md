```yaml
number: 5716
title: "Use `Cursor` for shebang parsing"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - performance
assignees: []
merged: true
base: main
head: charlie/cursor-ii
created_at: 2023-07-12T17:17:02Z
updated_at: 2023-07-12T21:39:36Z
url: https://github.com/astral-sh/ruff/pull/5716
synced_at: 2026-01-12T15:55:19Z
```

# Use `Cursor` for shebang parsing

---

_@charliermarsh_

## Summary

Better to leverage the shared functionality we get from `Cursor`. It's also a little bit faster, which is very cool.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-12 17:17_

---

_Label `internal` added by @charliermarsh on 2023-07-12 17:17_

---

_Label `performance` added by @charliermarsh on 2023-07-12 17:17_

---

_Comment by @github-actions[bot] on 2023-07-12 17:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.0±0.02ms     5.1 MB/sec    1.00      7.9±0.02ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1871.7±3.09µs     8.9 MB/sec    1.00   1866.0±8.47µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    209.1±0.31µs    14.1 MB/sec    1.00    208.9±0.22µs    14.1 MB/sec
formatter/pydantic/types.py                1.02      4.1±0.01ms     6.3 MB/sec    1.00      4.0±0.01ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.06ms     3.0 MB/sec    1.00     13.6±0.06ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    430.0±1.50µs     6.9 MB/sec    1.00    430.9±0.74µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.05ms     4.2 MB/sec    1.00      6.0±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.1 MB/sec    1.00      6.7±0.02ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1450.0±3.49µs    11.5 MB/sec    1.00   1453.0±3.59µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.5±0.51µs    17.9 MB/sec    1.00    164.4±0.86µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.0±0.27ms     4.5 MB/sec    1.00      9.0±0.29ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.07ms     8.2 MB/sec    1.00      2.0±0.09ms     8.1 MB/sec
formatter/numpy/globals.py                 1.00   222.1±10.31µs    13.3 MB/sec    1.02   227.5±10.75µs    13.0 MB/sec
formatter/pydantic/types.py                1.00      4.5±0.18ms     5.7 MB/sec    1.01      4.5±0.20ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     14.4±0.22ms     2.8 MB/sec    1.08     15.5±0.49ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.21ms     4.4 MB/sec    1.09      4.1±0.22ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    450.1±7.32µs     6.6 MB/sec    1.01   452.4±11.63µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.09ms     4.0 MB/sec    1.02      6.5±0.23ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.29ms     5.4 MB/sec    1.03      7.7±0.26ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1610.1±42.43µs    10.3 MB/sec    1.00  1587.5±53.16µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    185.6±9.48µs    15.9 MB/sec    1.02   190.2±28.66µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.05      3.4±0.07ms     7.4 MB/sec    1.00      3.3±0.05ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-07-12 20:52_

---

_Merged by @charliermarsh on 2023-07-12 21:22_

---

_Closed by @charliermarsh on 2023-07-12 21:22_

---

_Branch deleted on 2023-07-12 21:22_

---
