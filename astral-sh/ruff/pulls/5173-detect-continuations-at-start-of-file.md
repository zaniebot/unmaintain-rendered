```yaml
number: 5173
title: Detect continuations at start-of-file
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/continue
created_at: 2023-06-19T03:45:47Z
updated_at: 2023-06-19T04:20:42Z
url: https://github.com/astral-sh/ruff/pull/5173
synced_at: 2026-01-12T03:43:30Z
```

# Detect continuations at start-of-file

---

_Pull request opened by @charliermarsh on 2023-06-19 03:45_

## Summary

Given:

```python
\
import os
```

Deleting `import os` leaves a syntax error: a file can't end in a continuation. We have code to handle this case, but it failed to pick up continuations at the _very start_ of a file.

Closes #5156.


---

_Label `bug` added by @charliermarsh on 2023-06-19 03:45_

---

_Comment by @github-actions[bot] on 2023-06-19 04:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.3±0.02ms     6.5 MB/sec    1.00      6.3±0.04ms     6.5 MB/sec
formatter/numpy/ctypeslib.py               1.00  1312.0±10.40µs    12.7 MB/sec    1.00   1313.6±7.53µs    12.7 MB/sec
formatter/numpy/globals.py                 1.00    127.8±0.44µs    23.1 MB/sec    1.00    127.9±3.19µs    23.1 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.02ms     9.9 MB/sec    1.00      2.6±0.01ms     9.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.4±0.06ms     3.0 MB/sec    1.00     13.3±0.04ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    413.4±1.94µs     7.1 MB/sec    1.01    417.5±0.54µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.8±0.02ms     4.4 MB/sec    1.00      5.7±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.01ms     6.1 MB/sec    1.01      6.7±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1444.3±3.60µs    11.5 MB/sec    1.02   1467.7±8.50µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.3±2.70µs    18.6 MB/sec    1.03    162.6±0.31µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.01      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.9±0.30ms     5.1 MB/sec    1.01      8.0±0.24ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1623.9±68.65µs    10.3 MB/sec    1.01  1633.6±44.07µs    10.2 MB/sec
formatter/numpy/globals.py                 1.00    152.7±3.54µs    19.3 MB/sec    1.05    159.9±8.92µs    18.5 MB/sec
formatter/pydantic/types.py                1.00      3.3±0.10ms     7.8 MB/sec    1.05      3.4±0.14ms     7.5 MB/sec
linter/all-rules/large/dataset.py          1.00     16.5±0.47ms     2.5 MB/sec    1.02     16.8±0.46ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.12ms     3.9 MB/sec    1.02      4.3±0.14ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   498.2±14.09µs     5.9 MB/sec    1.00   496.2±10.73µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.27ms     3.5 MB/sec    1.01      7.3±0.24ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.02      8.7±0.14ms     4.7 MB/sec    1.00      8.5±0.15ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1780.6±33.98µs     9.4 MB/sec    1.01  1802.2±48.68µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.01    206.9±8.80µs    14.3 MB/sec    1.00    204.8±7.21µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.06ms     6.7 MB/sec    1.02      3.9±0.13ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-19 04:09_

---

_Closed by @charliermarsh on 2023-06-19 04:09_

---

_Branch deleted on 2023-06-19 04:09_

---
