```yaml
number: 5345
title: Remove HashMap and HashSet for known-standard-library detection
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/match
created_at: 2023-06-23T19:09:00Z
updated_at: 2023-06-23T20:17:54Z
url: https://github.com/astral-sh/ruff/pull/5345
synced_at: 2026-01-12T15:55:18Z
```

# Remove HashMap and HashSet for known-standard-library detection

---

_@charliermarsh_

## Summary

This is a lot more concise and probably much more performant (with fewer instructions).

---

_Merged by @charliermarsh on 2023-06-23 19:59_

---

_Closed by @charliermarsh on 2023-06-23 19:59_

---

_Branch deleted on 2023-06-23 19:59_

---

_Comment by @github-actions[bot] on 2023-06-23 20:02_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.0±0.03ms     5.8 MB/sec    1.00      7.0±0.01ms     5.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   1568.8±1.89µs    10.6 MB/sec    1.00   1571.3±2.26µs    10.6 MB/sec
formatter/numpy/globals.py                 1.01    180.8±0.20µs    16.3 MB/sec    1.00    179.7±0.21µs    16.4 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.01ms     7.1 MB/sec    1.00      3.6±0.01ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.00     13.7±0.07ms     3.0 MB/sec    1.02     13.9±0.07ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.8 MB/sec    1.02      3.5±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    364.7±1.82µs     8.1 MB/sec    1.00    365.4±1.15µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.02ms     4.2 MB/sec    1.01      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.03ms     5.8 MB/sec    1.01      7.1±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1478.2±7.71µs    11.3 MB/sec    1.02  1507.3±11.48µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.9±0.22µs    18.6 MB/sec    1.01    160.3±0.22µs    18.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.0 MB/sec    1.01      3.2±0.00ms     7.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.15      8.9±0.05ms     4.6 MB/sec    1.00      7.8±0.07ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.10  1966.7±25.70µs     8.5 MB/sec    1.00  1792.6±36.83µs     9.3 MB/sec
formatter/numpy/globals.py                 1.04    232.2±4.97µs    12.7 MB/sec    1.00    223.2±9.11µs    13.2 MB/sec
formatter/pydantic/types.py                1.10      4.5±0.07ms     5.7 MB/sec    1.00      4.0±0.05ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.1±0.16ms     2.7 MB/sec    1.00     15.1±0.10ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.06ms     4.1 MB/sec    1.00      4.1±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    498.9±5.81µs     5.9 MB/sec    1.00    500.7±5.63µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.06ms     3.8 MB/sec    1.00      6.8±0.06ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.05ms     5.2 MB/sec    1.00      7.9±0.05ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1724.8±21.03µs     9.7 MB/sec    1.00  1722.2±16.42µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.2±4.27µs    14.5 MB/sec    1.01    205.0±4.33µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.1 MB/sec    1.00      3.6±0.03ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
