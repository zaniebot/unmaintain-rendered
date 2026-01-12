```yaml
number: 6255
title: "Rename `Parameter#arg` and `ParameterWithDefault#def` fields"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/names
created_at: 2023-08-01T17:57:51Z
updated_at: 2023-08-01T18:32:18Z
url: https://github.com/astral-sh/ruff/pull/6255
synced_at: 2026-01-12T15:55:21Z
```

# Rename `Parameter#arg` and `ParameterWithDefault#def` fields

---

_@charliermarsh_

## Summary

This PR renames...

- `Parameter#arg` to `Parameter#name`
- `ParameterWithDefault#def` to `ParameterWithDefault#parameter` (such that `ParameterWithDefault` has a `default` and a `parameter`)

## Test Plan

`cargo test`


---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-01 17:57_

---

_Review requested from @zanieb by @charliermarsh on 2023-08-01 17:57_

---

_Comment by @github-actions[bot] on 2023-08-01 18:11_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.14     10.9±0.66ms     3.7 MB/sec    1.00      9.6±0.40ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.06      2.1±0.54ms     7.8 MB/sec    1.00      2.0±0.13ms     8.3 MB/sec
formatter/numpy/globals.py                 1.07   223.9±15.89µs    13.2 MB/sec    1.00   210.0±14.19µs    14.0 MB/sec
formatter/pydantic/types.py                1.16      4.7±0.31ms     5.5 MB/sec    1.00      4.0±0.17ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.09     14.7±0.53ms     2.8 MB/sec    1.00     13.6±0.55ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.11      3.7±0.20ms     4.5 MB/sec    1.00      3.4±0.11ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.04   492.0±23.96µs     6.0 MB/sec    1.00   472.9±22.45µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.08      6.5±0.25ms     3.9 MB/sec    1.00      6.0±0.21ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.06      7.6±0.31ms     5.4 MB/sec    1.00      7.2±0.23ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.07  1582.4±66.70µs    10.5 MB/sec    1.00  1476.9±57.98µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.05    181.8±9.85µs    16.2 MB/sec    1.00    173.6±9.57µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.05      3.3±0.16ms     7.8 MB/sec    1.00      3.1±0.09ms     8.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.08     11.6±0.50ms     3.5 MB/sec    1.00     10.7±0.54ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.05      2.2±0.11ms     7.5 MB/sec    1.00      2.1±0.15ms     7.9 MB/sec
formatter/numpy/globals.py                 1.03   258.1±19.06µs    11.4 MB/sec    1.00   250.8±21.53µs    11.8 MB/sec
formatter/pydantic/types.py                1.07      4.9±0.21ms     5.2 MB/sec    1.00      4.6±0.26ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.04     16.1±0.43ms     2.5 MB/sec    1.00     15.5±0.57ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      4.3±0.20ms     3.9 MB/sec    1.00      4.1±0.19ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.03   520.2±30.62µs     5.7 MB/sec    1.00   503.0±29.37µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.03      7.2±0.24ms     3.5 MB/sec    1.00      7.0±0.27ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.02      8.7±0.37ms     4.7 MB/sec    1.00      8.6±0.60ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.07  1774.0±88.22µs     9.4 MB/sec    1.00  1657.1±77.73µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.08   215.5±11.58µs    13.7 MB/sec    1.00   200.2±14.86µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.08      3.8±0.11ms     6.7 MB/sec    1.00      3.5±0.16ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb approved on 2023-08-01 18:28_

---

_Merged by @charliermarsh on 2023-08-01 18:28_

---

_Closed by @charliermarsh on 2023-08-01 18:28_

---

_Branch deleted on 2023-08-01 18:28_

---
