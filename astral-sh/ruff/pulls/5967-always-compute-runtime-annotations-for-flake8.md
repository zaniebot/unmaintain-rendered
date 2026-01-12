```yaml
number: 5967
title: Always compute runtime annotations for flake8-type-checking rules
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/strict
created_at: 2023-07-22T03:39:10Z
updated_at: 2023-07-22T04:06:13Z
url: https://github.com/astral-sh/ruff/pull/5967
synced_at: 2026-01-12T15:55:20Z
```

# Always compute runtime annotations for flake8-type-checking rules

---

_@charliermarsh_

## Summary

These are skipped as an optimization, but it feels kind of unnecessary and makes the code a bit more confusing than is worthwhile. (non-`strict` is also by far the more popular setting, and the default.)

---

_Label `internal` added by @charliermarsh on 2023-07-22 03:39_

---

_Marked ready for review by @charliermarsh on 2023-07-22 03:39_

---

_Comment by @github-actions[bot] on 2023-07-22 03:49_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.02ms     4.3 MB/sec    1.01      9.5±0.04ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1861.1±2.55µs     8.9 MB/sec    1.01   1870.9±3.15µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    203.8±0.37µs    14.5 MB/sec    1.01    205.5±4.40µs    14.4 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.3 MB/sec    1.02      4.1±0.01ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.01     13.0±0.05ms     3.1 MB/sec    1.00     12.9±0.06ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.00ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    436.9±1.01µs     6.8 MB/sec    1.00    434.6±0.71µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.9±0.01ms     4.3 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.02      6.6±0.01ms     6.2 MB/sec    1.00      6.5±0.01ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1415.7±2.72µs    11.8 MB/sec    1.00   1394.2±1.34µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    156.2±0.29µs    18.9 MB/sec    1.00    154.7±0.69µs    19.1 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.0±0.01ms     8.5 MB/sec    1.00      3.0±0.00ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.9±0.04ms     3.7 MB/sec    1.05     11.5±0.07ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.02ms     7.9 MB/sec    1.04      2.2±0.02ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00    228.9±2.12µs    12.9 MB/sec    1.04    238.1±8.59µs    12.4 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.02ms     5.5 MB/sec    1.04      4.9±0.03ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.00     15.2±0.06ms     2.7 MB/sec    1.00     15.2±0.06ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.02ms     4.2 MB/sec    1.00      4.0±0.02ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    399.5±4.97µs     7.4 MB/sec    1.01    402.5±5.42µs     7.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.02ms     3.7 MB/sec    1.00      6.9±0.03ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.03ms     5.1 MB/sec    1.01      8.0±0.03ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1614.4±60.93µs    10.3 MB/sec    1.00   1608.0±8.91µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.1±0.91µs    17.9 MB/sec    1.01    166.3±2.00µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.02ms     7.2 MB/sec    1.00      3.6±0.01ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-22 03:53_

---

_Closed by @charliermarsh on 2023-07-22 03:53_

---

_Branch deleted on 2023-07-22 03:53_

---
