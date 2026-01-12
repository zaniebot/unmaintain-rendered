```yaml
number: 6146
title: Add DTZ002 documentation
type: pull_request
state: merged
author: klistwan
labels:
  - documentation
assignees: []
merged: true
base: main
head: DTZ002-docs
created_at: 2023-07-28T07:17:35Z
updated_at: 2023-08-01T05:49:21Z
url: https://github.com/astral-sh/ruff/pull/6146
synced_at: 2026-01-12T02:58:30Z
```

# Add DTZ002 documentation

---

_Pull request opened by @klistwan on 2023-07-28 07:17_

## Summary

Adds documentation for DTZ002. Related to https://github.com/astral-sh/ruff/issues/2646.

## Test Plan

`python scripts/test_docs_formatted.py`


---

_Comment by @github-actions[bot] on 2023-07-28 07:31_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.6±0.13ms     4.7 MB/sec    1.00      8.5±0.13ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.01  1683.8±31.52µs     9.9 MB/sec    1.00  1660.4±27.09µs    10.0 MB/sec
formatter/numpy/globals.py                 1.01    181.9±2.91µs    16.2 MB/sec    1.00    179.3±2.05µs    16.5 MB/sec
formatter/pydantic/types.py                1.04      3.7±0.11ms     6.9 MB/sec    1.00      3.6±0.02ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.02     11.5±0.13ms     3.5 MB/sec    1.00     11.3±0.08ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      2.8±0.01ms     5.8 MB/sec    1.00      2.8±0.01ms     5.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    386.1±0.93µs     7.6 MB/sec    1.00    382.9±1.60µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.02      5.1±0.06ms     5.0 MB/sec    1.00      5.0±0.02ms     5.1 MB/sec
linter/default-rules/large/dataset.py      1.01      6.0±0.03ms     6.7 MB/sec    1.00      5.9±0.02ms     6.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1258.2±22.41µs    13.2 MB/sec    1.00   1244.1±4.02µs    13.4 MB/sec
linter/default-rules/numpy/globals.py      1.01    136.5±1.26µs    21.6 MB/sec    1.00    135.5±0.36µs    21.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.6±0.01ms     9.6 MB/sec    1.00      2.6±0.02ms     9.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.0±0.07ms     4.1 MB/sec    1.01     10.1±0.09ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1929.6±24.95µs     8.6 MB/sec    1.01  1950.8±20.68µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    212.8±8.76µs    13.9 MB/sec    1.02    217.2±7.00µs    13.6 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.04ms     6.0 MB/sec    1.01      4.3±0.05ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.3±0.08ms     3.1 MB/sec    1.00     13.3±0.09ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.06ms     4.7 MB/sec    1.01      3.5±0.03ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    432.9±6.72µs     6.8 MB/sec    1.01    435.6±6.57µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.06ms     4.3 MB/sec    1.01      6.0±0.08ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.06ms     5.5 MB/sec    1.00      7.4±0.05ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1489.5±12.70µs    11.2 MB/sec    1.02  1513.5±13.77µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.9±1.99µs    17.8 MB/sec    1.01    168.1±3.30µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.03ms     7.9 MB/sec    1.01      3.2±0.04ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @charliermarsh by @MichaReiser on 2023-07-28 07:51_

---

_Label `documentation` added by @charliermarsh on 2023-07-29 00:31_

---

_Comment by @charliermarsh on 2023-08-01 03:54_

Thanks!

---

_Merged by @charliermarsh on 2023-08-01 04:00_

---

_Closed by @charliermarsh on 2023-08-01 04:00_

---

_Branch deleted on 2023-08-01 05:49_

---
