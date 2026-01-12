```yaml
number: 4446
title: Add LightGBM to user list
type: pull_request
state: merged
author: jameslamb
labels: []
assignees: []
merged: true
base: main
head: docs/lightgbm
created_at: 2023-05-16T03:55:01Z
updated_at: 2023-05-16T04:33:18Z
url: https://github.com/astral-sh/ruff/pull/4446
synced_at: 2026-01-12T15:55:15Z
```

# Add LightGBM to user list

---

_@jameslamb_

We adopted `ruff` into our CI over in LightGBM tonight: https://github.com/microsoft/LightGBM/pull/5871

This PR proposes adding LightGBM to the list of large open-source projects using `ruff` in the README, as a small thank you for the excellent tool you've created here 😊 

---

_Comment by @charliermarsh on 2023-05-16 04:00_

Amazing, thank you for this! LightGBM is such a great project, so cool to see it on this list.

---

_Merged by @charliermarsh on 2023-05-16 04:04_

---

_Closed by @charliermarsh on 2023-05-16 04:04_

---

_Comment by @github-actions[bot] on 2023-05-16 04:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.00     16.6±0.92ms     2.4 MB/sec     1.11     18.5±0.74ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.22ms     4.2 MB/sec     1.11      4.4±0.17ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   507.0±35.65µs     5.8 MB/sec     1.07   544.8±19.25µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.32ms     3.8 MB/sec     1.14      7.7±0.29ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.40ms     5.2 MB/sec     1.12      8.8±0.24ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1770.8±101.05µs     9.4 MB/sec    1.05  1863.1±67.91µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.04   230.2±16.16µs    12.8 MB/sec     1.00   221.5±11.65µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.14ms     6.5 MB/sec     1.02      4.0±0.26ms     6.4 MB/sec
parser/large/dataset.py                    1.03      7.2±0.19ms     5.6 MB/sec     1.00      7.0±0.44ms     5.8 MB/sec
parser/numpy/ctypeslib.py                  1.00  1332.5±63.32µs    12.5 MB/sec     1.02  1365.3±51.02µs    12.2 MB/sec
parser/numpy/globals.py                    1.00    130.1±9.42µs    22.7 MB/sec     1.05    136.3±6.43µs    21.6 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.12ms     9.1 MB/sec     1.12      3.1±0.17ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.6±0.20ms     2.5 MB/sec    1.00     16.5±0.17ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.08ms     3.9 MB/sec    1.00      4.2±0.07ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    490.6±8.65µs     6.0 MB/sec    1.01    497.7±9.77µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.15ms     3.7 MB/sec    1.01      7.0±0.11ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.05ms     5.0 MB/sec    1.01      8.2±0.07ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1711.6±21.20µs     9.7 MB/sec    1.01  1734.3±29.29µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    188.7±3.47µs    15.6 MB/sec    1.03    194.9±8.11µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.0 MB/sec    1.01      3.7±0.06ms     6.9 MB/sec
parser/large/dataset.py                    1.01      6.7±0.05ms     6.1 MB/sec    1.00      6.6±0.05ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.01  1276.0±20.45µs    13.0 MB/sec    1.00  1267.9±19.88µs    13.1 MB/sec
parser/numpy/globals.py                    1.00    130.3±2.02µs    22.6 MB/sec    1.00    130.0±2.29µs    22.7 MB/sec
parser/pydantic/types.py                   1.01      2.9±0.03ms     8.9 MB/sec    1.00      2.8±0.03ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Branch deleted on 2023-05-16 04:33_

---
