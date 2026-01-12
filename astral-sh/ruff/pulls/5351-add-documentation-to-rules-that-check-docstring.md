```yaml
number: 5351
title: "Add documentation to rules that check docstring quotes (`D3XX`)"
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: docstring-quotes
created_at: 2023-06-24T11:17:00Z
updated_at: 2023-07-10T09:55:16Z
url: https://github.com/astral-sh/ruff/pull/5351
synced_at: 2026-01-12T03:36:55Z
```

# Add documentation to rules that check docstring quotes (`D3XX`)

---

_Pull request opened by @tjkuson on 2023-06-24 11:17_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Add documentation to the `D3XX` rules that check for issues with docstring quotes. Related to #2646.

## Test Plan

`python scripts/check_docs_formatted.py`


---

_Comment by @github-actions[bot] on 2023-06-24 11:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.0±0.01ms     5.8 MB/sec    1.02      7.1±0.01ms     5.7 MB/sec
formatter/numpy/ctypeslib.py               1.00   1568.5±2.35µs    10.6 MB/sec    1.02   1597.1±2.04µs    10.4 MB/sec
formatter/numpy/globals.py                 1.00    179.2±0.25µs    16.5 MB/sec    1.02    182.6±0.25µs    16.2 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.01ms     7.1 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
linter/all-rules/large/dataset.py          1.06     14.4±0.02ms     2.8 MB/sec    1.00     13.6±0.02ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      3.6±0.01ms     4.6 MB/sec    1.00      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.03    373.4±1.36µs     7.9 MB/sec    1.00    363.4±0.96µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.05      6.3±0.01ms     4.0 MB/sec    1.00      6.0±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.10      7.8±0.01ms     5.2 MB/sec    1.00      7.1±0.02ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.08   1601.1±3.43µs    10.4 MB/sec    1.00   1481.9±3.15µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.07    167.0±0.35µs    17.7 MB/sec    1.00    156.7±0.21µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.08      3.5±0.01ms     7.4 MB/sec    1.00      3.2±0.00ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.9±0.11ms     5.2 MB/sec    1.00      7.8±0.06ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1799.4±20.79µs     9.3 MB/sec    1.00  1804.0±38.98µs     9.2 MB/sec
formatter/numpy/globals.py                 1.00    220.1±6.88µs    13.4 MB/sec    1.04   229.7±21.48µs    12.8 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.06ms     6.3 MB/sec    1.00      4.1±0.06ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.01     15.5±0.16ms     2.6 MB/sec    1.00     15.3±0.15ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.04ms     4.1 MB/sec    1.00      4.1±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01   505.9±10.72µs     5.8 MB/sec    1.00    498.6±6.10µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.0±0.16ms     3.7 MB/sec    1.00      6.8±0.09ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.01      8.0±0.06ms     5.1 MB/sec    1.00      7.9±0.06ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1735.6±15.15µs     9.6 MB/sec    1.00  1711.1±24.48µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    201.4±6.01µs    14.7 MB/sec    1.00    200.5±4.27µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.03ms     7.0 MB/sec    1.00      3.6±0.05ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-06-25 22:22_

---

_Merged by @charliermarsh on 2023-06-25 22:34_

---

_Closed by @charliermarsh on 2023-06-25 22:34_

---

_Branch deleted on 2023-07-10 09:55_

---
