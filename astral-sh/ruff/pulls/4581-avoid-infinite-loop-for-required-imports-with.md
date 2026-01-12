```yaml
number: 4581
title: "Avoid infinite loop for required imports with isort: off"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/isort-off
created_at: 2023-05-22T15:35:44Z
updated_at: 2023-05-22T16:18:57Z
url: https://github.com/astral-sh/ruff/pull/4581
synced_at: 2026-01-12T03:50:03Z
```

# Avoid infinite loop for required imports with isort: off

---

_Pull request opened by @charliermarsh on 2023-05-22 15:35_

## Summary

When determining whether the required import exists in the file, we're ignoring any exclusion blocks. This PR decouples the required import rule from the import block-tracking logic, since we only care about top-level imports anyway.

Closes #4575.


---

_Merged by @charliermarsh on 2023-05-22 15:49_

---

_Closed by @charliermarsh on 2023-05-22 15:49_

---

_Branch deleted on 2023-05-22 15:49_

---

_Comment by @github-actions[bot] on 2023-05-22 16:01_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.8±0.05ms     2.8 MB/sec    1.00     14.7±0.06ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.00ms     4.6 MB/sec    1.00      3.6±0.00ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    365.1±1.51µs     8.1 MB/sec    1.01    368.3±1.21µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.02ms     4.1 MB/sec    1.00      6.1±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.04ms     5.6 MB/sec    1.00      7.2±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1515.2±5.52µs    11.0 MB/sec    1.01   1525.6±5.67µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.9±0.28µs    18.1 MB/sec    1.01    165.3±1.54µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.00ms     7.8 MB/sec    1.01      3.3±0.01ms     7.8 MB/sec
parser/large/dataset.py                    1.01      5.7±0.01ms     7.1 MB/sec    1.00      5.7±0.00ms     7.2 MB/sec
parser/numpy/ctypeslib.py                  1.02   1135.0±0.77µs    14.7 MB/sec    1.00   1117.1±2.21µs    14.9 MB/sec
parser/numpy/globals.py                    1.03    116.9±0.20µs    25.2 MB/sec    1.00    113.7±0.89µs    25.9 MB/sec
parser/pydantic/types.py                   1.02      2.5±0.00ms    10.3 MB/sec    1.00      2.4±0.00ms    10.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     23.6±0.91ms  1768.4 KB/sec    1.00     23.3±1.01ms  1789.7 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.9±0.30ms     2.8 MB/sec    1.02      6.0±0.37ms     2.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   686.4±83.39µs     4.3 MB/sec    1.00   686.7±48.18µs     4.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.7±0.44ms     2.6 MB/sec    1.03     10.0±0.42ms     2.6 MB/sec
linter/default-rules/large/dataset.py      1.00     11.4±0.44ms     3.6 MB/sec    1.00     11.4±0.42ms     3.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.4±0.12ms     7.0 MB/sec    1.04      2.5±0.11ms     6.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   282.5±15.02µs    10.4 MB/sec    1.00   283.2±20.51µs    10.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.1±0.22ms     5.0 MB/sec    1.03      5.2±0.23ms     4.9 MB/sec
parser/large/dataset.py                    1.00      9.5±1.45ms     4.3 MB/sec    1.17     11.1±0.50ms     3.7 MB/sec
parser/numpy/ctypeslib.py                  1.00  1801.8±82.40µs     9.2 MB/sec    1.17      2.1±0.08ms     7.9 MB/sec
parser/numpy/globals.py                    1.00    181.9±9.28µs    16.2 MB/sec    1.13   206.0±12.07µs    14.3 MB/sec
parser/pydantic/types.py                   1.00      4.0±0.17ms     6.4 MB/sec    1.16      4.6±0.19ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
