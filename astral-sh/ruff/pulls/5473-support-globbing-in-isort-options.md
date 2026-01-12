```yaml
number: 5473
title: "Support globbing in `isort` options"
type: pull_request
state: merged
author: tjkuson
labels:
  - isort
  - configuration
assignees: []
merged: true
base: main
head: glob
created_at: 2023-07-03T09:44:58Z
updated_at: 2023-07-10T09:54:47Z
url: https://github.com/astral-sh/ruff/pull/5473
synced_at: 2026-01-12T15:55:18Z
```

# Support globbing in `isort` options

---

_@tjkuson_

## Summary

Support glob patterns in `isort` options.

Closes #5420.

## Test Plan

Added test.

`cargo test`

---

_Comment by @github-actions[bot] on 2023-07-03 09:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      8.9±0.24ms     4.6 MB/sec    1.00      8.8±0.26ms     4.6 MB/sec
formatter/numpy/ctypeslib.py               1.01  1915.3±42.78µs     8.7 MB/sec    1.00  1905.4±52.24µs     8.7 MB/sec
formatter/numpy/globals.py                 1.12   239.5±14.16µs    12.3 MB/sec    1.00    213.4±7.94µs    13.8 MB/sec
formatter/pydantic/types.py                1.04      4.4±0.14ms     5.8 MB/sec    1.00      4.2±0.19ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     14.4±0.68ms     2.8 MB/sec    1.00     14.4±0.79ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      3.8±0.17ms     4.4 MB/sec    1.00      3.6±0.17ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.07   484.4±21.83µs     6.1 MB/sec    1.00   453.7±18.49µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.32ms     4.0 MB/sec    1.01      6.5±0.27ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.28ms     5.6 MB/sec    1.02      7.4±0.22ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1594.8±38.44µs    10.4 MB/sec    1.03  1648.3±73.85µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    186.7±5.08µs    15.8 MB/sec    1.00    184.8±5.53µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.3±0.07ms     7.7 MB/sec    1.00      3.2±0.09ms     7.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.7±0.05ms     4.7 MB/sec    1.01      8.8±0.26ms     4.6 MB/sec
formatter/numpy/ctypeslib.py               1.00   1791.7±6.99µs     9.3 MB/sec    1.00  1794.2±12.55µs     9.3 MB/sec
formatter/numpy/globals.py                 1.01    194.1±0.91µs    15.2 MB/sec    1.00    193.0±1.88µs    15.3 MB/sec
formatter/pydantic/types.py                1.01      4.0±0.01ms     6.4 MB/sec    1.00      4.0±0.05ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     14.1±0.06ms     2.9 MB/sec    1.01     14.2±0.17ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.8 MB/sec    1.00      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    391.9±1.98µs     7.5 MB/sec    1.01    395.2±7.13µs     7.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.04ms     4.2 MB/sec    1.01      6.2±0.13ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.05ms     5.5 MB/sec    1.00      7.4±0.20ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1478.8±60.02µs    11.3 MB/sec    1.01  1494.7±22.18µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.8±0.79µs    17.9 MB/sec    1.03    169.3±1.70µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.0 MB/sec    1.02      3.2±0.06ms     7.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @tjkuson on 2023-07-03 10:15_

---

_Review requested from @charliermarsh by @charliermarsh on 2023-07-05 13:56_

---

_Label `isort` added by @charliermarsh on 2023-07-05 20:36_

---

_Label `configuration` added by @charliermarsh on 2023-07-05 20:36_

---

_Comment by @charliermarsh on 2023-07-06 22:57_

Working on testing + merging this.

---

_Merged by @charliermarsh on 2023-07-07 00:37_

---

_Closed by @charliermarsh on 2023-07-07 00:37_

---

_Comment by @charliermarsh on 2023-07-07 00:39_

Thanks!

---

_Branch deleted on 2023-07-10 09:54_

---
