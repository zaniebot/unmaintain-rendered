```yaml
number: 4977
title: "Respect 'is not' operators split across newlines"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/located-ops
created_at: 2023-06-09T04:57:27Z
updated_at: 2023-06-09T05:27:45Z
url: https://github.com/astral-sh/ruff/pull/4977
synced_at: 2026-01-12T03:43:29Z
```

# Respect 'is not' operators split across newlines

---

_Pull request opened by @charliermarsh on 2023-06-09 04:57_

Closes #4973.

---

_Label `bug` added by @charliermarsh on 2023-06-09 04:57_

---

_Merged by @charliermarsh on 2023-06-09 05:07_

---

_Closed by @charliermarsh on 2023-06-09 05:07_

---

_Branch deleted on 2023-06-09 05:07_

---

_Comment by @github-actions[bot] on 2023-06-09 05:11_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.44ms     4.9 MB/sec    1.03      8.5±0.48ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.03  1553.2±89.21µs    10.7 MB/sec    1.00  1508.1±116.91µs    11.0 MB/sec
formatter/numpy/globals.py                 1.00   170.1±15.49µs    17.3 MB/sec    1.01   172.4±21.22µs    17.1 MB/sec
formatter/pydantic/types.py                1.05      3.6±0.29ms     7.1 MB/sec    1.00      3.4±0.23ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.04     18.2±0.86ms     2.2 MB/sec    1.00     17.6±0.74ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.20ms     3.8 MB/sec    1.00      4.4±0.23ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   544.2±27.75µs     5.4 MB/sec    1.03   561.2±28.95µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.08      8.2±0.57ms     3.1 MB/sec    1.00      7.6±0.57ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.04      8.7±0.35ms     4.7 MB/sec    1.00      8.4±0.39ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1825.0±90.49µs     9.1 MB/sec    1.02  1853.7±89.62µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.05   226.9±13.21µs    13.0 MB/sec    1.00   215.5±17.22µs    13.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.21ms     6.3 MB/sec    1.02      4.1±0.31ms     6.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      9.8±0.39ms     4.2 MB/sec    1.00      9.6±0.32ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.01  1688.0±83.49µs     9.9 MB/sec    1.00  1667.7±71.36µs    10.0 MB/sec
formatter/numpy/globals.py                 1.00   185.1±12.76µs    15.9 MB/sec    1.02   188.1±12.71µs    15.7 MB/sec
formatter/pydantic/types.py                1.00      3.9±0.16ms     6.5 MB/sec    1.03      4.0±0.16ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     20.6±0.61ms  2025.2 KB/sec    1.02     20.9±0.67ms  1991.1 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.3±0.22ms     3.2 MB/sec    1.01      5.3±0.22ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.04   630.6±37.47µs     4.7 MB/sec    1.00   608.3±32.63µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.04      9.1±0.40ms     2.8 MB/sec    1.00      8.7±0.32ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.3±0.32ms     4.0 MB/sec    1.00     10.3±0.35ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.3±0.13ms     7.4 MB/sec    1.00      2.2±0.11ms     7.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   245.6±11.68µs    12.0 MB/sec    1.03   252.6±14.25µs    11.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.25ms     5.5 MB/sec    1.02      4.7±0.20ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
