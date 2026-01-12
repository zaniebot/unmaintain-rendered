```yaml
number: 6238
title: "Fix `logger-objects` documentation"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/logger
created_at: 2023-08-01T12:47:46Z
updated_at: 2023-08-01T13:25:12Z
url: https://github.com/astral-sh/ruff/pull/6238
synced_at: 2026-01-12T02:58:30Z
```

# Fix `logger-objects` documentation

---

_Pull request opened by @charliermarsh on 2023-08-01 12:47_

Closes https://github.com/astral-sh/ruff/issues/6234.

---

_Label `documentation` added by @charliermarsh on 2023-08-01 12:50_

---

_Merged by @charliermarsh on 2023-08-01 12:57_

---

_Closed by @charliermarsh on 2023-08-01 12:57_

---

_Branch deleted on 2023-08-01 12:57_

---

_Comment by @github-actions[bot] on 2023-08-01 13:01_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.4±0.04ms     4.9 MB/sec    1.00      8.3±0.06ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1629.7±14.71µs    10.2 MB/sec    1.01  1648.0±32.74µs    10.1 MB/sec
formatter/numpy/globals.py                 1.03    177.7±6.42µs    16.6 MB/sec    1.00    173.1±1.79µs    17.0 MB/sec
formatter/pydantic/types.py                1.01      3.6±0.02ms     7.1 MB/sec    1.00      3.5±0.02ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.00     11.3±0.03ms     3.6 MB/sec    1.00     11.3±0.02ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.01ms     5.7 MB/sec    1.00      2.9±0.01ms     5.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    318.0±0.96µs     9.3 MB/sec    1.00    317.8±6.00µs     9.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.1±0.02ms     5.0 MB/sec    1.00      5.1±0.03ms     5.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.0±0.01ms     6.8 MB/sec    1.01      6.0±0.01ms     6.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1194.4±8.35µs    13.9 MB/sec    1.00   1198.6±5.11µs    13.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    121.9±0.70µs    24.2 MB/sec    1.00    121.6±0.26µs    24.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.6±0.01ms     9.7 MB/sec    1.01      2.7±0.01ms     9.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.0±0.15ms     4.1 MB/sec    1.00      9.9±0.10ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1937.5±33.18µs     8.6 MB/sec    1.00  1929.4±31.03µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    215.0±4.53µs    13.7 MB/sec    1.01    216.4±9.38µs    13.6 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.05ms     6.0 MB/sec    1.00      4.3±0.07ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.19ms     3.0 MB/sec    1.01     13.7±0.14ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.04ms     4.7 MB/sec    1.02      3.6±0.03ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    434.4±7.36µs     6.8 MB/sec    1.02    443.6±8.69µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.07ms     4.2 MB/sec    1.01      6.1±0.09ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.13ms     5.5 MB/sec    1.00      7.5±0.08ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1501.1±15.50µs    11.1 MB/sec    1.02  1526.6±23.95µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    167.7±2.87µs    17.6 MB/sec    1.02    170.8±2.31µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.04ms     7.9 MB/sec    1.01      3.3±0.04ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
