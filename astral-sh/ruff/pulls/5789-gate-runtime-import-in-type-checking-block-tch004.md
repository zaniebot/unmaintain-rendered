```yaml
number: 5789
title: "Gate `runtime-import-in-type-checking-block` (`TCH004`) behind enabled flag"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/gate
created_at: 2023-07-15T20:50:54Z
updated_at: 2023-07-15T21:23:39Z
url: https://github.com/astral-sh/ruff/pull/5789
synced_at: 2026-01-12T03:30:21Z
```

# Gate `runtime-import-in-type-checking-block` (`TCH004`) behind enabled flag

---

_Pull request opened by @charliermarsh on 2023-07-15 20:50_

## Summary

This rule is running whenever _any_ `TCH` rules are enabled, which is not intentional.

Closes #5787.

---

_Renamed from "Gate RuntimeImportInTypeCheckingBlock behind enabled flag" to "Gate `runtime-import-in-type-checking-block` (`TCH004`) behind enabled flag" by @charliermarsh on 2023-07-15 20:51_

---

_Merged by @charliermarsh on 2023-07-15 20:57_

---

_Closed by @charliermarsh on 2023-07-15 20:57_

---

_Branch deleted on 2023-07-15 20:57_

---

_Comment by @github-actions[bot] on 2023-07-15 21:02_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.9±0.03ms     4.1 MB/sec    1.00      9.8±0.08ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.01  1903.0±10.84µs     8.8 MB/sec    1.00   1878.3±7.19µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    205.5±0.74µs    14.4 MB/sec    1.00    205.4±2.78µs    14.4 MB/sec
formatter/pydantic/types.py                1.03      4.3±0.01ms     6.0 MB/sec    1.00      4.1±0.00ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.01     14.2±0.03ms     2.9 MB/sec    1.00     14.0±0.02ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    381.0±0.87µs     7.7 MB/sec    1.01    386.2±9.99µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.01ms     4.1 MB/sec    1.00      6.3±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.02ms     5.7 MB/sec    1.00      7.1±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1461.9±2.40µs    11.4 MB/sec    1.00   1466.4±4.81µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    156.3±0.24µs    18.9 MB/sec    1.01    157.4±0.32µs    18.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.0 MB/sec    1.00      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.9±0.09ms     3.7 MB/sec    1.01     10.9±0.11ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.03ms     7.8 MB/sec    1.00      2.1±0.03ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    243.4±4.78µs    12.1 MB/sec    1.02    247.1±7.84µs    11.9 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.06ms     5.5 MB/sec    1.00      4.7±0.11ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.9±0.15ms     2.6 MB/sec    1.00     15.8±0.14ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.0 MB/sec    1.00      4.1±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    501.1±6.51µs     5.9 MB/sec    1.00    498.7±7.27µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.09ms     3.6 MB/sec    1.00      7.1±0.08ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.04ms     5.1 MB/sec    1.01      8.0±0.07ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1705.3±32.08µs     9.8 MB/sec    1.00  1706.9±25.19µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.6±3.39µs    14.7 MB/sec    1.00    199.9±5.13µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.1 MB/sec    1.00      3.6±0.04ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
