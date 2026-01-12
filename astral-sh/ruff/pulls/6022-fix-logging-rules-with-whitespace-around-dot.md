```yaml
number: 6022
title: Fix logging rules with whitespace around dot
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/logging-call
created_at: 2023-07-24T04:53:10Z
updated_at: 2023-07-24T05:34:06Z
url: https://github.com/astral-sh/ruff/pull/6022
synced_at: 2026-01-12T15:55:20Z
```

# Fix logging rules with whitespace around dot

---

_@charliermarsh_

## Summary

Attempting to fix, e.g., `logging . warn("Hello World!")` was causing a syntax error.

---

_Label `bug` added by @charliermarsh on 2023-07-24 04:53_

---

_Comment by @github-actions[bot] on 2023-07-24 05:02_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.3±0.03ms     4.4 MB/sec    1.00      9.2±0.02ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1861.7±9.43µs     8.9 MB/sec    1.00   1856.6±2.75µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    208.8±0.37µs    14.1 MB/sec    1.01    210.5±3.93µs    14.0 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.4 MB/sec    1.00      4.0±0.01ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.0±0.08ms     3.1 MB/sec    1.01     13.1±0.08ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.00ms     5.1 MB/sec    1.00      3.3±0.00ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    435.8±0.76µs     6.8 MB/sec    1.00    434.6±0.65µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.02ms     4.4 MB/sec    1.00      5.9±0.04ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.01      6.6±0.03ms     6.2 MB/sec    1.00      6.6±0.02ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1418.3±3.44µs    11.7 MB/sec    1.00   1414.1±1.99µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.01    159.2±0.52µs    18.5 MB/sec    1.00    157.9±0.21µs    18.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.01ms     8.6 MB/sec    1.00      3.0±0.00ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     11.3±0.13ms     3.6 MB/sec    1.00     11.1±0.14ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.2±0.03ms     7.6 MB/sec    1.00      2.2±0.03ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00    243.5±4.43µs    12.1 MB/sec    1.02    247.3±8.05µs    11.9 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.06ms     5.4 MB/sec    1.01      4.8±0.08ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.9±0.13ms     2.6 MB/sec    1.00     15.9±0.12ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.08ms     4.0 MB/sec    1.00      4.1±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    492.3±6.89µs     6.0 MB/sec    1.00    490.8±9.00µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.10ms     3.5 MB/sec    1.00      7.2±0.08ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.07ms     5.0 MB/sec    1.00      8.1±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1698.0±21.81µs     9.8 MB/sec    1.00  1677.6±16.25µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.7±4.45µs    15.3 MB/sec    1.00    192.1±8.81µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.04ms     7.0 MB/sec    1.00      3.6±0.03ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-24 05:14_

---

_Closed by @charliermarsh on 2023-07-24 05:14_

---

_Branch deleted on 2023-07-24 05:14_

---
