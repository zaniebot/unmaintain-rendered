```yaml
number: 5109
title: Add autofix specification levels for a variety of rules
type: pull_request
state: merged
author: charliermarsh
labels:
  - fixes
assignees: []
merged: true
base: main
head: charlie/spec
created_at: 2023-06-15T01:49:24Z
updated_at: 2023-06-15T02:16:25Z
url: https://github.com/astral-sh/ruff/pull/5109
synced_at: 2026-01-12T03:43:30Z
```

# Add autofix specification levels for a variety of rules

---

_Pull request opened by @charliermarsh on 2023-06-15 01:49_

## Summary

Just idly going through these while I watch baseball :)

---

_Label `autofix` added by @charliermarsh on 2023-06-15 01:49_

---

_Comment by @github-actions[bot] on 2023-06-15 02:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.01ms     6.0 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   1386.9±3.22µs    12.0 MB/sec    1.00   1388.7±2.86µs    12.0 MB/sec
formatter/numpy/globals.py                 1.00    136.9±1.00µs    21.5 MB/sec    1.01    138.2±0.83µs    21.4 MB/sec
formatter/pydantic/types.py                1.00      2.7±0.01ms     9.3 MB/sec    1.01      2.8±0.03ms     9.2 MB/sec
linter/all-rules/large/dataset.py          1.01     14.3±0.08ms     2.8 MB/sec    1.00     14.2±0.06ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.00ms     4.7 MB/sec    1.00      3.5±0.00ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    365.5±0.85µs     8.1 MB/sec    1.00    364.9±1.71µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.03ms     4.1 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.01ms     5.7 MB/sec    1.00      7.1±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1509.9±3.48µs    11.0 MB/sec    1.00   1509.0±4.69µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    165.4±1.65µs    17.8 MB/sec    1.00    164.4±0.15µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.8 MB/sec    1.00      3.3±0.01ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.9±0.08ms     5.9 MB/sec    1.02      7.1±0.13ms     5.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1406.9±25.06µs    11.8 MB/sec    1.00  1412.7±28.15µs    11.8 MB/sec
formatter/numpy/globals.py                 1.00    132.6±4.09µs    22.3 MB/sec    1.03    136.9±4.27µs    21.6 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.07ms     9.0 MB/sec    1.01      2.9±0.07ms     8.9 MB/sec
linter/all-rules/large/dataset.py          1.00     14.9±0.30ms     2.7 MB/sec    1.02     15.2±0.36ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.05ms     4.5 MB/sec    1.03      3.8±0.06ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   451.8±16.28µs     6.5 MB/sec    1.00    450.0±9.36µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.15ms     4.0 MB/sec    1.01      6.5±0.13ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.15ms     5.5 MB/sec    1.01      7.5±0.11ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1574.2±26.97µs    10.6 MB/sec    1.01  1592.9±29.63µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    176.0±6.08µs    16.8 MB/sec    1.01    177.3±4.14µs    16.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.06ms     7.6 MB/sec    1.02      3.4±0.06ms     7.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-15 02:03_

---

_Closed by @charliermarsh on 2023-06-15 02:03_

---

_Branch deleted on 2023-06-15 02:03_

---
