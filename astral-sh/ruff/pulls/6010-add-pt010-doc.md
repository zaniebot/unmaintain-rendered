```yaml
number: 6010
title: "Add `PT010` doc"
type: pull_request
state: merged
author: harupy
labels:
  - documentation
assignees: []
merged: true
base: main
head: PT010-doc
created_at: 2023-07-23T08:04:15Z
updated_at: 2023-07-24T02:06:51Z
url: https://github.com/astral-sh/ruff/pull/6010
synced_at: 2026-01-12T03:30:22Z
```

# Add `PT010` doc

---

_Pull request opened by @harupy on 2023-07-23 08:04_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

#2646

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-07-23 08:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.5±0.42ms     3.9 MB/sec    1.04     10.9±0.28ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.1±0.11ms     7.9 MB/sec    1.00      2.1±0.09ms     8.0 MB/sec
formatter/numpy/globals.py                 1.00   242.0±12.57µs    12.2 MB/sec    1.07   258.9±17.92µs    11.4 MB/sec
formatter/pydantic/types.py                1.00      4.6±0.15ms     5.6 MB/sec    1.04      4.7±0.20ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.57ms     2.6 MB/sec    1.03     15.8±0.81ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.13ms     4.3 MB/sec    1.07      4.2±0.09ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.02   523.7±22.16µs     5.6 MB/sec    1.00   512.8±21.47µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.23ms     3.7 MB/sec    1.04      7.3±0.32ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.02      7.9±0.23ms     5.1 MB/sec    1.00      7.8±0.20ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1655.5±63.29µs    10.1 MB/sec    1.02  1696.9±51.80µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.1±8.24µs    15.4 MB/sec    1.01    194.6±8.60µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.11ms     7.2 MB/sec    1.03      3.7±0.13ms     6.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.06      8.6±0.16ms     4.7 MB/sec    1.00      8.1±0.16ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.07  1665.7±58.12µs    10.0 MB/sec    1.00  1558.4±39.89µs    10.7 MB/sec
formatter/numpy/globals.py                 1.03    174.4±4.70µs    16.9 MB/sec    1.00    170.0±5.84µs    17.4 MB/sec
formatter/pydantic/types.py                1.05      3.7±0.08ms     6.9 MB/sec    1.00      3.5±0.09ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.01     11.2±0.13ms     3.6 MB/sec    1.00     11.1±0.12ms     3.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.0±0.02ms     5.6 MB/sec    1.00      2.9±0.03ms     5.7 MB/sec
linter/all-rules/numpy/globals.py          1.04    305.7±6.83µs     9.7 MB/sec    1.00    293.2±5.45µs    10.1 MB/sec
linter/all-rules/pydantic/types.py         1.02      5.1±0.07ms     5.0 MB/sec    1.00      5.0±0.05ms     5.1 MB/sec
linter/default-rules/large/dataset.py      1.03      6.0±0.11ms     6.7 MB/sec    1.00      5.8±0.04ms     7.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1209.9±28.27µs    13.8 MB/sec    1.00   1164.0±9.92µs    14.3 MB/sec
linter/default-rules/numpy/globals.py      1.04    124.6±1.52µs    23.7 MB/sec    1.00    120.0±1.60µs    24.6 MB/sec
linter/default-rules/pydantic/types.py     1.02      2.7±0.07ms     9.6 MB/sec    1.00      2.6±0.06ms     9.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-07-24 01:36_

---

_Merged by @charliermarsh on 2023-07-24 01:43_

---

_Closed by @charliermarsh on 2023-07-24 01:43_

---
