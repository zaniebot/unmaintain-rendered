```yaml
number: 6596
title: Fix PT005 doc
type: pull_request
state: merged
author: harupy
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-PT005-doc
created_at: 2023-08-15T12:39:03Z
updated_at: 2023-08-15T13:11:00Z
url: https://github.com/astral-sh/ruff/pull/6596
synced_at: 2026-01-12T02:52:04Z
```

# Fix PT005 doc

---

_Pull request opened by @harupy on 2023-08-15 12:39_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

#2646

The docstring location is incorrect.

## Test Plan

<!-- How was it tested? -->

<img width="874" alt="image" src="https://github.com/astral-sh/ruff/assets/17039389/c78b3300-ba1f-42c2-aef7-9305ef6ed9e0">


---

_Label `documentation` added by @charliermarsh on 2023-08-15 12:41_

---

_Merged by @charliermarsh on 2023-08-15 12:48_

---

_Closed by @charliermarsh on 2023-08-15 12:48_

---

_Comment by @github-actions[bot] on 2023-08-15 12:52_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.05      4.8±0.04ms     8.5 MB/sec    1.00      4.5±0.07ms     9.0 MB/sec
formatter/numpy/ctypeslib.py               1.06    958.0±3.95µs    17.4 MB/sec    1.00   901.3±10.52µs    18.5 MB/sec
formatter/numpy/globals.py                 1.07     98.0±0.50µs    30.1 MB/sec    1.00     91.7±3.82µs    32.2 MB/sec
formatter/pydantic/types.py                1.06  1922.4±42.30µs    13.3 MB/sec    1.00  1820.2±26.31µs    14.0 MB/sec
linter/all-rules/large/dataset.py          1.00     12.2±0.18ms     3.3 MB/sec    1.02     12.5±0.11ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.05ms     5.1 MB/sec    1.03      3.3±0.02ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    470.3±9.29µs     6.3 MB/sec    1.01    473.9±0.97µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.06ms     4.0 MB/sec    1.01      6.4±0.02ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.05ms     6.2 MB/sec    1.01      6.6±0.04ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1444.5±4.93µs    11.5 MB/sec    1.01   1459.6±9.53µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.01    168.7±0.37µs    17.5 MB/sec    1.00    167.0±4.92µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.01ms     8.7 MB/sec    1.00      3.0±0.02ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.02      5.0±0.31ms     8.1 MB/sec     1.00      4.9±0.21ms     8.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   935.5±38.59µs    17.8 MB/sec     1.01   942.2±33.26µs    17.7 MB/sec
formatter/numpy/globals.py                 1.00     90.0±3.75µs    32.8 MB/sec     1.05     94.4±5.75µs    31.2 MB/sec
formatter/pydantic/types.py                1.00  1899.3±102.37µs    13.4 MB/sec    1.02  1936.6±87.20µs    13.2 MB/sec
linter/all-rules/large/dataset.py          1.00     15.1±0.42ms     2.7 MB/sec     1.01     15.4±0.42ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.16ms     4.0 MB/sec     1.00      4.2±0.12ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   528.6±19.07µs     5.6 MB/sec     1.03   542.0±19.54µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.9±0.21ms     3.2 MB/sec     1.00      7.8±0.16ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.17ms     4.9 MB/sec     1.00      8.3±0.23ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1830.4±48.47µs     9.1 MB/sec     1.00  1772.1±47.46µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    223.5±7.08µs    13.2 MB/sec     1.03    229.2±9.25µs    12.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.11ms     6.7 MB/sec     1.00      3.8±0.09ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
