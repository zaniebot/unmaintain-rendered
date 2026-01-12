```yaml
number: 6130
title: "Add documentation to the `flynt` rules"
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: flynt-docs
created_at: 2023-07-27T16:08:32Z
updated_at: 2023-07-28T09:14:28Z
url: https://github.com/astral-sh/ruff/pull/6130
synced_at: 2026-01-12T15:55:20Z
```

# Add documentation to the `flynt` rules

---

_@tjkuson_

## Summary

Completes the documentation for the one and only (current) rule in the `flynt` ruleset. Related to #2646.

## Test Plan

`python scripts/test_docs_formatted.py`

---

_Renamed from "Add more documentation to the `flynt` rules" to "Add documentation to the `flynt` rules" by @tjkuson on 2023-07-27 16:10_

---

_Comment by @github-actions[bot] on 2023-07-27 16:40_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.1±0.10ms     4.5 MB/sec    1.00      9.0±0.05ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   1741.4±5.73µs     9.6 MB/sec    1.00  1734.5±23.48µs     9.6 MB/sec
formatter/numpy/globals.py                 1.01    183.3±0.22µs    16.1 MB/sec    1.00    181.0±0.20µs    16.3 MB/sec
formatter/pydantic/types.py                1.01      3.8±0.01ms     6.7 MB/sec    1.00      3.8±0.02ms     6.7 MB/sec
linter/all-rules/large/dataset.py          1.00     12.8±0.13ms     3.2 MB/sec    1.01     12.9±0.06ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.00ms     5.2 MB/sec    1.01      3.2±0.02ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    340.7±1.75µs     8.7 MB/sec    1.01    345.6±4.64µs     8.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.7±0.04ms     4.4 MB/sec    1.00      5.7±0.03ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.4±0.04ms     6.3 MB/sec    1.01      6.5±0.02ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1282.1±3.23µs    13.0 MB/sec    1.00   1286.6±4.82µs    12.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    127.5±2.82µs    23.1 MB/sec    1.00    125.9±1.52µs    23.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.00ms     9.1 MB/sec    1.00      2.8±0.01ms     9.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.9±0.36ms     3.2 MB/sec    1.00     12.9±0.37ms     3.2 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.4±0.09ms     6.8 MB/sec    1.00      2.4±0.08ms     6.9 MB/sec
formatter/numpy/globals.py                 1.00   268.9±15.93µs    11.0 MB/sec    1.00   268.2±22.27µs    11.0 MB/sec
formatter/pydantic/types.py                1.00      5.2±0.13ms     4.9 MB/sec    1.02      5.3±0.18ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.00     18.7±0.38ms     2.2 MB/sec    1.01     18.9±0.50ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.22ms     3.5 MB/sec    1.01      4.9±0.17ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   564.2±23.37µs     5.2 MB/sec    1.00   566.6±17.83µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.3±0.28ms     3.1 MB/sec    1.01      8.4±0.22ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.5±0.24ms     4.3 MB/sec    1.01      9.6±0.26ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1906.9±98.56µs     8.7 MB/sec    1.00  1914.7±47.92µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.02   211.5±12.41µs    13.9 MB/sec    1.00    207.1±7.40µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.14ms     6.2 MB/sec    1.01      4.2±0.18ms     6.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-07-27 18:32_

---

_Merged by @charliermarsh on 2023-07-27 18:32_

---

_Closed by @charliermarsh on 2023-07-27 18:32_

---

_Branch deleted on 2023-07-28 09:14_

---
