```yaml
number: 6703
title: "Refactor `flake8_pytest_style/rules/parametrize.rs`"
type: pull_request
state: merged
author: harupy
labels:
  - internal
assignees: []
merged: true
base: main
head: refactor-parametrize
created_at: 2023-08-20T13:49:04Z
updated_at: 2023-08-20T14:50:36Z
url: https://github.com/astral-sh/ruff/pull/6703
synced_at: 2026-01-12T02:52:04Z
```

# Refactor `flake8_pytest_style/rules/parametrize.rs`

---

_Pull request opened by @harupy on 2023-08-20 13:49_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Minor refactoring in `flake8_pytest_style/rules/parametrize.rs`.

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-08-20 14:01_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.07      3.5±0.11ms    11.5 MB/sec    1.00      3.3±0.19ms    12.3 MB/sec
formatter/numpy/ctypeslib.py               1.16   779.6±25.82µs    21.4 MB/sec    1.00   673.3±39.51µs    24.7 MB/sec
formatter/numpy/globals.py                 1.19     84.0±1.83µs    35.1 MB/sec    1.00     70.5±4.01µs    41.9 MB/sec
formatter/pydantic/types.py                1.22  1551.3±49.79µs    16.4 MB/sec    1.00  1268.9±53.93µs    20.1 MB/sec
linter/all-rules/large/dataset.py          1.00     10.1±0.41ms     4.0 MB/sec    1.10     11.1±0.44ms     3.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      3.1±0.12ms     5.4 MB/sec    1.00      3.0±0.14ms     5.6 MB/sec
linter/all-rules/numpy/globals.py          1.09   434.5±13.19µs     6.8 MB/sec    1.00   400.3±29.61µs     7.4 MB/sec
linter/all-rules/pydantic/types.py         1.05      5.6±0.29ms     4.5 MB/sec    1.00      5.3±0.26ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.01      5.9±0.20ms     6.8 MB/sec    1.00      5.9±0.27ms     6.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.09  1382.9±36.11µs    12.0 MB/sec    1.00  1270.0±78.94µs    13.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    145.7±6.65µs    20.3 MB/sec    1.00    143.6±8.87µs    20.5 MB/sec
linter/default-rules/pydantic/types.py     1.02      2.5±0.11ms    10.4 MB/sec    1.00      2.4±0.09ms    10.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.3±0.07ms    12.2 MB/sec    1.01      3.4±0.08ms    12.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   690.4±34.65µs    24.1 MB/sec    1.00   692.1±24.86µs    24.1 MB/sec
formatter/numpy/globals.py                 1.00     70.4±2.24µs    41.9 MB/sec    1.01     71.1±1.84µs    41.5 MB/sec
formatter/pydantic/types.py                1.00  1388.1±48.04µs    18.4 MB/sec    1.01  1398.5±47.03µs    18.2 MB/sec
linter/all-rules/large/dataset.py          1.00     11.5±0.18ms     3.5 MB/sec    1.00     11.5±0.16ms     3.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.1±0.07ms     5.3 MB/sec    1.00      3.2±0.12ms     5.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    391.3±6.98µs     7.5 MB/sec    1.00    391.2±7.68µs     7.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.13ms     4.3 MB/sec    1.00      6.0±0.12ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.3±0.08ms     6.4 MB/sec    1.01      6.4±0.11ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1331.4±19.88µs    12.5 MB/sec    1.01  1345.0±28.73µs    12.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    154.2±3.58µs    19.1 MB/sec    1.00    154.3±3.16µs    19.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.04ms     9.0 MB/sec    1.02      2.9±0.05ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-08-20 14:17_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pytest_style/rules/parametrize.rs`:639 on 2023-08-20 14:17_

If we're in the business of minor refactors, I tend to prefer `args.as_slice()` for this. I just find it a bit clearer.

---

_Label `internal` added by @charliermarsh on 2023-08-20 14:21_

---

_Merged by @charliermarsh on 2023-08-20 14:30_

---

_Closed by @charliermarsh on 2023-08-20 14:30_

---

_Branch deleted on 2023-08-20 14:32_

---
