```yaml
number: 5968
title: "Move `locator`, `stylist`, and friends better getters"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/accessors
created_at: 2023-07-22T03:52:29Z
updated_at: 2023-07-22T13:37:25Z
url: https://github.com/astral-sh/ruff/pull/5968
synced_at: 2026-01-12T03:30:22Z
```

# Move `locator`, `stylist`, and friends better getters

---

_Pull request opened by @charliermarsh on 2023-07-22 03:52_

## Summary

Rather than exposing these as public fields, use getters, similar to `semantic()`.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-22 03:52_

---

_Label `internal` added by @charliermarsh on 2023-07-22 03:52_

---

_Comment by @github-actions[bot] on 2023-07-22 04:04_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.7±0.03ms     4.2 MB/sec    1.10     10.7±0.02ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   1896.4±2.93µs     8.8 MB/sec    1.08      2.0±0.00ms     8.1 MB/sec
formatter/numpy/globals.py                 1.00    206.3±1.84µs    14.3 MB/sec    1.05    216.3±0.55µs    13.6 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.01ms     6.1 MB/sec    1.08      4.5±0.02ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.00     13.5±0.01ms     3.0 MB/sec    1.00     13.6±0.02ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.8 MB/sec    1.01      3.5±0.00ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    373.0±0.90µs     7.9 MB/sec    1.01    375.3±1.02µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.02ms     5.8 MB/sec    1.01      7.1±0.01ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1427.5±2.32µs    11.7 MB/sec    1.00   1432.9±5.33µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    148.9±0.34µs    19.8 MB/sec    1.01    150.6±0.34µs    19.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.00ms     8.2 MB/sec    1.01      3.1±0.01ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     11.2±0.14ms     3.6 MB/sec    1.00     11.1±0.13ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.04ms     7.6 MB/sec    1.00      2.2±0.05ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00    243.1±4.76µs    12.1 MB/sec    1.01    245.2±8.16µs    12.0 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.06ms     5.4 MB/sec    1.01      4.8±0.07ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.01     15.9±0.15ms     2.6 MB/sec    1.00     15.7±0.16ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.10ms     4.0 MB/sec    1.00      4.1±0.06ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    495.8±7.96µs     6.0 MB/sec    1.01    498.9±8.78µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.10ms     3.6 MB/sec    1.00      7.2±0.13ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.11ms     5.0 MB/sec    1.01      8.2±0.10ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1699.9±26.73µs     9.8 MB/sec    1.00  1703.3±22.46µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    190.6±3.79µs    15.5 MB/sec    1.00    191.5±3.23µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.05ms     7.1 MB/sec    1.01      3.6±0.03ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-07-22 10:33_

This makes me happy. Thank you.

---

_Merged by @charliermarsh on 2023-07-22 13:37_

---

_Closed by @charliermarsh on 2023-07-22 13:37_

---

_Branch deleted on 2023-07-22 13:37_

---
