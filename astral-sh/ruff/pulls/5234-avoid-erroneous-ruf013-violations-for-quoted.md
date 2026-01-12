```yaml
number: 5234
title: Avoid erroneous RUF013 violations for quoted annotations
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/quoted
created_at: 2023-06-21T01:21:28Z
updated_at: 2023-06-21T01:53:36Z
url: https://github.com/astral-sh/ruff/pull/5234
synced_at: 2026-01-12T15:55:18Z
```

# Avoid erroneous RUF013 violations for quoted annotations

---

_@charliermarsh_

## Summary

Temporary fix for #5231: if we can't flag and fix these properly, just disabling them for now.

\cc @dhruvmanila 

## Test Plan

`cargo test`


---

_Review requested from @dhruvmanila by @charliermarsh on 2023-06-21 01:21_

---

_Merged by @charliermarsh on 2023-06-21 01:29_

---

_Closed by @charliermarsh on 2023-06-21 01:29_

---

_Branch deleted on 2023-06-21 01:29_

---

_Comment by @github-actions[bot] on 2023-06-21 01:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.01ms     6.0 MB/sec    1.01      6.8±0.01ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.01   1375.8±2.94µs    12.1 MB/sec    1.00   1365.3±1.97µs    12.2 MB/sec
formatter/numpy/globals.py                 1.00    132.5±0.40µs    22.3 MB/sec    1.00    133.0±0.81µs    22.2 MB/sec
formatter/pydantic/types.py                1.01      2.8±0.02ms     9.2 MB/sec    1.00      2.8±0.01ms     9.2 MB/sec
linter/all-rules/large/dataset.py          1.00     13.7±0.01ms     3.0 MB/sec    1.00     13.7±0.02ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.8 MB/sec    1.00      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.02    368.3±7.84µs     8.0 MB/sec    1.00    362.2±1.24µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.01      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.01ms     5.7 MB/sec    1.00      7.1±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1488.9±4.72µs    11.2 MB/sec    1.00   1484.6±2.78µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    156.4±0.22µs    18.9 MB/sec    1.00    156.4±0.14µs    18.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.00ms     7.9 MB/sec    1.00      3.2±0.01ms     7.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.5±0.06ms     5.4 MB/sec    1.00      7.6±0.07ms     5.4 MB/sec
formatter/numpy/ctypeslib.py               1.00  1552.0±20.91µs    10.7 MB/sec    1.01  1565.8±31.55µs    10.6 MB/sec
formatter/numpy/globals.py                 1.00    154.5±3.05µs    19.1 MB/sec    1.02    156.9±8.24µs    18.8 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.04ms     8.1 MB/sec    1.00      3.1±0.06ms     8.1 MB/sec
linter/all-rules/large/dataset.py          1.00     15.5±0.21ms     2.6 MB/sec    1.00     15.5±0.16ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.07ms     4.1 MB/sec    1.01      4.1±0.06ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   498.6±11.53µs     5.9 MB/sec    1.01    505.7±8.30µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.07ms     3.7 MB/sec    1.03      7.0±0.08ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.07ms     5.1 MB/sec    1.02      8.1±0.09ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1703.9±18.19µs     9.8 MB/sec    1.00  1705.0±27.08µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.5±3.33µs    15.0 MB/sec    1.00    197.5±2.93µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.1 MB/sec    1.00      3.6±0.03ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
