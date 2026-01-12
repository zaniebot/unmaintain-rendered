```yaml
number: 6533
title: Fix minor document errors
type: pull_request
state: merged
author: takumaw
labels:
  - documentation
assignees: []
merged: true
base: main
head: main
created_at: 2023-08-13T08:33:07Z
updated_at: 2023-08-13T17:35:31Z
url: https://github.com/astral-sh/ruff/pull/6533
synced_at: 2026-01-12T15:55:21Z
```

# Fix minor document errors

---

_@takumaw_

## Summary

Fix minor errors in the sample codes of some rules.

## Test Plan

N/A (Just fix document typos.)

---

_Comment by @github-actions[bot] on 2023-08-13 08:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.4±0.01ms     4.8 MB/sec    1.00      8.4±0.02ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   1629.3±1.83µs    10.2 MB/sec    1.01   1638.8±2.42µs    10.2 MB/sec
formatter/numpy/globals.py                 1.00    171.9±0.26µs    17.2 MB/sec    1.01    173.0±1.88µs    17.1 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.01ms     7.2 MB/sec    1.00      3.6±0.01ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.00     10.5±0.05ms     3.9 MB/sec    1.00     10.5±0.02ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.00ms     5.8 MB/sec    1.00      2.9±0.00ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    322.9±2.75µs     9.1 MB/sec    1.00    322.3±1.08µs     9.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.4±0.01ms     4.7 MB/sec    1.00      5.5±0.01ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.02ms     7.4 MB/sec    1.00      5.5±0.01ms     7.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1168.1±3.25µs    14.3 MB/sec    1.00   1171.8±2.30µs    14.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    123.2±0.33µs    24.0 MB/sec    1.01    124.4±0.59µs    23.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.2 MB/sec    1.00      2.5±0.01ms    10.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.09ms     4.1 MB/sec    1.01     10.0±0.13ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1912.5±17.15µs     8.7 MB/sec    1.01  1924.2±19.28µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    214.5±4.16µs    13.8 MB/sec    1.01    216.9±7.84µs    13.6 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.05ms     6.1 MB/sec    1.01      4.2±0.05ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.01     12.6±0.09ms     3.2 MB/sec    1.00     12.6±0.11ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.03ms     4.8 MB/sec    1.00      3.5±0.02ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    441.1±6.10µs     6.7 MB/sec    1.00   442.9±12.73µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.06ms     3.9 MB/sec    1.01      6.6±0.11ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.06ms     5.9 MB/sec    1.00      6.9±0.09ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1468.7±14.51µs    11.3 MB/sec    1.01  1476.7±30.18µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    174.4±2.85µs    16.9 MB/sec    1.00    175.2±2.54µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.03ms     8.3 MB/sec    1.00      3.1±0.03ms     8.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-08-13 17:35_

---

_Comment by @charliermarsh on 2023-08-13 17:35_

Thanks!

---

_Merged by @charliermarsh on 2023-08-13 17:35_

---

_Closed by @charliermarsh on 2023-08-13 17:35_

---
