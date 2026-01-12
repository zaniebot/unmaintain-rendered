```yaml
number: 5131
title: Add Applicability to flake8_tidy_imports
type: pull_request
state: merged
author: evanrittenhouse
labels: []
assignees: []
merged: true
base: main
head: applicability_tidy_imports
created_at: 2023-06-15T21:06:25Z
updated_at: 2023-06-17T22:00:00Z
url: https://github.com/astral-sh/ruff/pull/5131
synced_at: 2026-01-12T03:43:30Z
```

# Add Applicability to flake8_tidy_imports

---

_Pull request opened by @evanrittenhouse on 2023-06-15 21:06_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Fixes some of https://github.com/astral-sh/ruff/issues/4184
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-06-15 21:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.01ms     6.0 MB/sec    1.01      6.9±0.01ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1407.2±3.02µs    11.8 MB/sec    1.00   1404.0±2.35µs    11.9 MB/sec
formatter/numpy/globals.py                 1.01    138.2±1.92µs    21.4 MB/sec    1.00    137.4±0.28µs    21.5 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.01ms     9.1 MB/sec    1.00      2.8±0.01ms     9.1 MB/sec
linter/all-rules/large/dataset.py          1.00     14.2±0.03ms     2.9 MB/sec    1.00     14.2±0.03ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.00ms     4.8 MB/sec    1.00      3.5±0.02ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    368.0±2.57µs     8.0 MB/sec    1.00    367.3±3.72µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.02ms     4.2 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.01ms     5.7 MB/sec    1.00      7.1±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1508.4±3.59µs    11.0 MB/sec    1.00   1511.6±3.12µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    165.5±4.55µs    17.8 MB/sec    1.00    164.7±0.22µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.00ms     7.8 MB/sec    1.00      3.3±0.01ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.9±0.05ms     5.2 MB/sec    1.00      7.8±0.05ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1585.6±11.93µs    10.5 MB/sec    1.00  1581.8±25.26µs    10.5 MB/sec
formatter/numpy/globals.py                 1.00    156.4±1.64µs    18.9 MB/sec    1.01    157.8±3.40µs    18.7 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.02ms     8.0 MB/sec    1.00      3.2±0.02ms     7.9 MB/sec
linter/all-rules/large/dataset.py          1.00     16.6±0.18ms     2.5 MB/sec    1.02     17.0±0.19ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.05ms     3.8 MB/sec    1.02      4.4±0.05ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    438.6±8.35µs     6.7 MB/sec    1.03    450.3±7.52µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.10ms     3.4 MB/sec    1.02      7.6±0.05ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.07ms     4.8 MB/sec    1.00      8.5±0.11ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1768.8±78.10µs     9.4 MB/sec    1.00  1751.2±15.24µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    190.0±2.98µs    15.5 MB/sec    1.00    189.8±5.97µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.02ms     6.7 MB/sec    1.00      3.8±0.02ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-15 22:09_

---

_Closed by @charliermarsh on 2023-06-15 22:09_

---

_Branch deleted on 2023-06-17 22:00_

---
