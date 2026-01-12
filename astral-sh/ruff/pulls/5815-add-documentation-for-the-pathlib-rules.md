```yaml
number: 5815
title: Add documentation for the pathlib rules
type: pull_request
state: merged
author: sbrugman
labels:
  - documentation
assignees: []
merged: true
base: main
head: pathlib-docs
created_at: 2023-07-16T23:36:59Z
updated_at: 2023-07-21T01:25:45Z
url: https://github.com/astral-sh/ruff/pull/5815
synced_at: 2026-01-12T15:55:19Z
```

# Add documentation for the pathlib rules

---

_@sbrugman_

Reviving https://github.com/astral-sh/ruff/pull/2348 step by step

Pt 1: docs

Tracking issue: https://github.com/astral-sh/ruff/issues/2646.

---

_Comment by @github-actions[bot] on 2023-07-16 23:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     11.1±0.05ms     3.7 MB/sec    1.00     11.0±0.05ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.2±0.00ms     7.5 MB/sec    1.00      2.2±0.01ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00    244.0±1.27µs    12.1 MB/sec    1.01    245.4±6.58µs    12.0 MB/sec
formatter/pydantic/types.py                1.01      4.8±0.01ms     5.3 MB/sec    1.00      4.8±0.02ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.5±0.11ms     2.6 MB/sec    1.01     15.6±0.09ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.02ms     4.2 MB/sec    1.01      4.0±0.01ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    520.5±3.08µs     5.7 MB/sec    1.00    521.9±0.90µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.05ms     3.6 MB/sec    1.00      7.0±0.03ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.03ms     5.2 MB/sec    1.00      7.9±0.04ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1694.9±8.38µs     9.8 MB/sec    1.00   1688.8±7.55µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    188.9±1.12µs    15.6 MB/sec    1.00    188.4±1.26µs    15.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.04ms     7.1 MB/sec    1.00      3.6±0.02ms     7.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.7±0.32ms     3.2 MB/sec    1.02     13.0±0.38ms     3.1 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.5±0.10ms     6.6 MB/sec    1.00      2.5±0.10ms     6.6 MB/sec
formatter/numpy/globals.py                 1.00   276.8±16.33µs    10.7 MB/sec    1.03   285.7±22.92µs    10.3 MB/sec
formatter/pydantic/types.py                1.01      5.5±0.19ms     4.6 MB/sec    1.00      5.5±0.19ms     4.6 MB/sec
linter/all-rules/large/dataset.py          1.01     18.3±0.44ms     2.2 MB/sec    1.00     18.1±0.38ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.14ms     3.5 MB/sec    1.02      4.8±0.18ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   570.9±21.25µs     5.2 MB/sec    1.02   584.8±18.46µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.4±0.25ms     3.0 MB/sec    1.00      8.4±0.20ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.5±0.22ms     4.3 MB/sec    1.00      9.5±0.23ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.0±0.10ms     8.3 MB/sec    1.00  1986.1±57.57µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    222.3±9.43µs    13.3 MB/sec    1.00   222.6±11.39µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.2±0.10ms     6.0 MB/sec    1.00      4.2±0.13ms     6.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Converted to draft by @sbrugman on 2023-07-17 09:34_

---

_Marked ready for review by @sbrugman on 2023-07-17 09:34_

---

_Renamed from "pathlib rule docs" to "Add documentation for the pathlib rules" by @sbrugman on 2023-07-17 09:38_

---

_Converted to draft by @sbrugman on 2023-07-20 16:22_

---

_Marked ready for review by @sbrugman on 2023-07-20 23:49_

---

_Label `documentation` added by @charliermarsh on 2023-07-21 00:56_

---

_Merged by @charliermarsh on 2023-07-21 01:02_

---

_Closed by @charliermarsh on 2023-07-21 01:02_

---

_Branch deleted on 2023-07-21 01:10_

---
