```yaml
number: 5260
title: Improve documentation for overlong-line rules
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/e501
created_at: 2023-06-21T16:54:22Z
updated_at: 2023-06-21T17:34:13Z
url: https://github.com/astral-sh/ruff/pull/5260
synced_at: 2026-01-12T03:43:30Z
```

# Improve documentation for overlong-line rules

---

_Pull request opened by @charliermarsh on 2023-06-21 16:54_

Closes https://github.com/astral-sh/ruff/issues/5248.

---

_Label `documentation` added by @charliermarsh on 2023-06-21 16:54_

---

_Merged by @charliermarsh on 2023-06-21 17:02_

---

_Closed by @charliermarsh on 2023-06-21 17:02_

---

_Branch deleted on 2023-06-21 17:02_

---

_Comment by @github-actions[bot] on 2023-06-21 17:21_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.5±0.05ms     6.3 MB/sec    1.00      6.5±0.03ms     6.3 MB/sec
formatter/numpy/ctypeslib.py               1.01   1343.1±1.94µs    12.4 MB/sec    1.00   1324.7±5.22µs    12.6 MB/sec
formatter/numpy/globals.py                 1.00    127.8±0.18µs    23.1 MB/sec    1.00    127.9±0.23µs    23.1 MB/sec
formatter/pydantic/types.py                1.01      2.7±0.02ms     9.4 MB/sec    1.00      2.7±0.02ms     9.5 MB/sec
linter/all-rules/large/dataset.py          1.00     12.9±0.05ms     3.1 MB/sec    1.00     13.0±0.06ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.1 MB/sec    1.01      3.3±0.04ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    418.5±0.96µs     7.1 MB/sec    1.00    416.8±0.97µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.7±0.03ms     4.5 MB/sec    1.01      5.7±0.04ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.01ms     6.3 MB/sec    1.01      6.6±0.03ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1424.6±2.11µs    11.7 MB/sec    1.01   1436.9±3.12µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    157.0±0.15µs    18.8 MB/sec    1.03    161.3±0.76µs    18.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.6 MB/sec    1.01      3.0±0.01ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03     12.5±0.45ms     3.3 MB/sec    1.00     12.1±0.42ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.3±0.10ms     7.2 MB/sec    1.01      2.3±0.15ms     7.1 MB/sec
formatter/numpy/globals.py                 1.00   241.9±16.67µs    12.2 MB/sec    1.03   248.0±17.11µs    11.9 MB/sec
formatter/pydantic/types.py                1.02      4.9±0.28ms     5.2 MB/sec    1.00      4.8±0.24ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.02     24.0±0.58ms  1739.3 KB/sec    1.00     23.4±0.84ms  1780.2 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      6.3±0.24ms     2.7 MB/sec    1.02      6.4±0.32ms     2.6 MB/sec
linter/all-rules/numpy/globals.py          1.01   722.4±36.35µs     4.1 MB/sec    1.00   716.4±34.80µs     4.1 MB/sec
linter/all-rules/pydantic/types.py         1.01     10.8±0.42ms     2.4 MB/sec    1.00     10.6±0.36ms     2.4 MB/sec
linter/default-rules/large/dataset.py      1.00     12.6±0.55ms     3.2 MB/sec    1.00     12.5±0.44ms     3.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.5±0.13ms     6.6 MB/sec    1.00      2.5±0.09ms     6.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   297.3±24.48µs     9.9 MB/sec    1.00   298.1±18.61µs     9.9 MB/sec
linter/default-rules/pydantic/types.py     1.02      5.5±0.18ms     4.6 MB/sec    1.00      5.4±0.18ms     4.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
