```yaml
number: 5135
title: "Minor formatting changes to `Checker`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/space
created_at: 2023-06-16T02:33:20Z
updated_at: 2023-06-16T02:57:58Z
url: https://github.com/astral-sh/ruff/pull/5135
synced_at: 2026-01-12T03:43:30Z
```

# Minor formatting changes to `Checker`

---

_Pull request opened by @charliermarsh on 2023-06-16 02:33_

_No description provided._

---

_Label `rule` added by @charliermarsh on 2023-06-16 02:33_

---

_Merged by @charliermarsh on 2023-06-16 02:42_

---

_Closed by @charliermarsh on 2023-06-16 02:42_

---

_Branch deleted on 2023-06-16 02:42_

---

_Comment by @github-actions[bot] on 2023-06-16 02:44_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.9±0.02ms     5.9 MB/sec    1.00      6.8±0.04ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.01   1404.8±3.77µs    11.9 MB/sec    1.00   1393.4±2.73µs    11.9 MB/sec
formatter/numpy/globals.py                 1.00    137.7±0.34µs    21.4 MB/sec    1.00    137.1±0.68µs    21.5 MB/sec
formatter/pydantic/types.py                1.01      2.8±0.01ms     9.1 MB/sec    1.00      2.8±0.01ms     9.2 MB/sec
linter/all-rules/large/dataset.py          1.01     14.5±0.13ms     2.8 MB/sec    1.00     14.4±0.17ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.03ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    368.6±0.95µs     8.0 MB/sec    1.00    368.5±1.52µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.04ms     4.1 MB/sec    1.00      6.2±0.04ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.03ms     5.7 MB/sec    1.00      7.2±0.10ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1519.1±5.12µs    11.0 MB/sec    1.00   1517.9±3.73µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.6±0.26µs    17.9 MB/sec    1.00    165.2±0.33µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.01ms     7.7 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.32     10.0±0.25ms     4.1 MB/sec    1.00      7.6±0.07ms     5.4 MB/sec
formatter/numpy/ctypeslib.py               1.32      2.1±0.30ms     8.1 MB/sec    1.00  1551.3±16.97µs    10.7 MB/sec
formatter/numpy/globals.py                 1.22   184.3±29.10µs    16.0 MB/sec    1.00    151.2±8.34µs    19.5 MB/sec
formatter/pydantic/types.py                1.33      4.2±0.52ms     6.1 MB/sec    1.00      3.1±0.08ms     8.1 MB/sec
linter/all-rules/large/dataset.py          1.07     20.1±0.98ms     2.0 MB/sec    1.00     18.8±1.29ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.23      5.4±0.55ms     3.1 MB/sec    1.00      4.4±0.32ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.26  663.8±123.90µs     4.4 MB/sec    1.00   527.1±63.31µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.13      9.1±0.61ms     2.8 MB/sec    1.00      8.1±0.87ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.24     10.8±0.88ms     3.8 MB/sec    1.00      8.7±0.68ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.20      2.3±0.28ms     7.1 MB/sec    1.00  1957.7±249.41µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.31   259.0±39.62µs    11.4 MB/sec    1.00    197.4±9.79µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.26      5.1±0.51ms     5.0 MB/sec    1.00      4.0±0.43ms     6.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
