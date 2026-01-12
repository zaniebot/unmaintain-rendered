```yaml
number: 5100
title: "Enable UTC-import for `datetime-utc-alias` fix"
type: pull_request
state: merged
author: charliermarsh
labels:
  - fixes
assignees: []
merged: true
base: main
head: charlie/utc
created_at: 2023-06-14T19:39:30Z
updated_at: 2023-06-14T21:37:02Z
url: https://github.com/astral-sh/ruff/pull/5100
synced_at: 2026-01-12T15:55:17Z
```

# Enable UTC-import for `datetime-utc-alias` fix

---

_@charliermarsh_

## Summary

Small update to leverage `get_or_import_symbol` to fix `UP017` in more cases (e.g., when we need to import `UTC`, or access it from an alias or something).

## Test Plan

Check out the updated snapshot.


---

_Label `autofix` added by @charliermarsh on 2023-06-14 19:39_

---

_Comment by @github-actions[bot] on 2023-06-14 19:50_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.3±0.05ms     6.5 MB/sec    1.00      6.3±0.02ms     6.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   1308.1±2.69µs    12.7 MB/sec    1.00   1306.5±4.37µs    12.7 MB/sec
formatter/numpy/globals.py                 1.00    124.9±0.17µs    23.6 MB/sec    1.04    129.6±4.41µs    22.8 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.01ms    10.0 MB/sec    1.00      2.6±0.01ms     9.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.11ms     3.0 MB/sec    1.01     13.7±0.10ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.1 MB/sec    1.01      3.3±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    411.6±0.90µs     7.2 MB/sec    1.01    415.9±3.43µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.02ms     4.4 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.02ms     6.2 MB/sec    1.01      6.6±0.03ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1422.7±5.32µs    11.7 MB/sec    1.01   1430.8±6.29µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    160.5±0.20µs    18.4 MB/sec    1.01    161.8±0.35µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.01      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.9±0.04ms     5.2 MB/sec    1.00      7.8±0.09ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1586.5±9.84µs    10.5 MB/sec    1.00  1583.4±35.05µs    10.5 MB/sec
formatter/numpy/globals.py                 1.00    156.1±2.00µs    18.9 MB/sec    1.01    157.3±4.48µs    18.8 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.03ms     7.9 MB/sec    1.00      3.2±0.13ms     8.0 MB/sec
linter/all-rules/large/dataset.py          1.00     16.4±0.12ms     2.5 MB/sec    1.00     16.5±0.16ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.06ms     3.9 MB/sec    1.00      4.2±0.04ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    435.6±6.71µs     6.8 MB/sec    1.00   437.6±11.93µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.15ms     3.5 MB/sec    1.00      7.2±0.05ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.05ms     4.8 MB/sec    1.00      8.4±0.05ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1756.9±18.12µs     9.5 MB/sec    1.00  1746.8±16.15µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    188.6±2.08µs    15.6 MB/sec    1.00    188.9±1.61µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.03ms     6.7 MB/sec    1.00      3.8±0.01ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-14 21:13_

---

_Closed by @charliermarsh on 2023-06-14 21:13_

---

_Branch deleted on 2023-06-14 21:13_

---
