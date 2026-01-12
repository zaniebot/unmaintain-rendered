```yaml
number: 4721
title: "Move `fixable` checks into patch blocks"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/remove-fixable
created_at: 2023-05-30T01:56:21Z
updated_at: 2023-05-30T02:25:47Z
url: https://github.com/astral-sh/ruff/pull/4721
synced_at: 2026-01-12T15:55:16Z
```

# Move `fixable` checks into patch blocks

---

_@charliermarsh_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Now that we "always generate fixes", we no longer need the bespoke `fixable` property that we previously computed and stored on diagnostics in a one-off way. Instead, we can just do the `fixable` computation as-needed within the fix block itself.

## Test Plan

`cargo test`


---

_Renamed from "Move fixable checks into patch blocks" to "Move `fixable` checks into patch blocks" by @charliermarsh on 2023-05-30 01:56_

---

_@charliermarsh reviewed on 2023-05-30 01:58_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pytest_style/rules/assertion.rs`:188 on 2023-05-30 01:58_

(Every change here is mechanical: take the RHS of `let fixable =`, remove the assignment, and move the check into the `if checker.patch` block, which is then indented.)

---

_Label `internal` added by @charliermarsh on 2023-05-30 01:58_

---

_Merged by @charliermarsh on 2023-05-30 02:09_

---

_Closed by @charliermarsh on 2023-05-30 02:09_

---

_Branch deleted on 2023-05-30 02:09_

---

_Comment by @github-actions[bot] on 2023-05-30 02:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.2±0.09ms     2.4 MB/sec    1.00     17.2±0.14ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.02ms     4.0 MB/sec    1.00      4.1±0.01ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    512.1±1.05µs     5.8 MB/sec    1.00    508.9±1.03µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.04ms     3.6 MB/sec    1.00      7.1±0.03ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.02ms     5.0 MB/sec    1.00      8.2±0.03ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1793.2±8.90µs     9.3 MB/sec    1.00   1796.8±2.15µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    201.8±0.32µs    14.6 MB/sec    1.02    205.0±2.46µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     6.9 MB/sec    1.00      3.7±0.01ms     6.9 MB/sec
parser/large/dataset.py                    1.01      6.3±0.02ms     6.4 MB/sec    1.00      6.3±0.01ms     6.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1244.9±0.78µs    13.4 MB/sec    1.00   1240.0±4.99µs    13.4 MB/sec
parser/numpy/globals.py                    1.02    129.9±6.36µs    22.7 MB/sec    1.00    127.8±0.25µs    23.1 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.00ms     9.4 MB/sec    1.00      2.7±0.01ms     9.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.0±0.29ms     2.4 MB/sec    1.00     16.9±0.25ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.05ms     3.9 MB/sec    1.00      4.3±0.10ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01   505.6±11.11µs     5.8 MB/sec    1.00    498.1±5.70µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.2±0.09ms     3.6 MB/sec    1.00      7.0±0.09ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.3±0.12ms     4.9 MB/sec    1.00      8.2±0.05ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1785.0±27.51µs     9.3 MB/sec    1.00  1768.4±31.26µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.4±2.94µs    14.5 MB/sec    1.02    207.2±5.11µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.7 MB/sec    1.00      3.8±0.05ms     6.7 MB/sec
parser/large/dataset.py                    1.07      7.0±0.05ms     5.8 MB/sec    1.00      6.5±0.08ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.05  1299.0±11.47µs    12.8 MB/sec    1.00  1233.1±14.17µs    13.5 MB/sec
parser/numpy/globals.py                    1.04    130.7±1.78µs    22.6 MB/sec    1.00    125.7±2.18µs    23.5 MB/sec
parser/pydantic/types.py                   1.06      2.9±0.03ms     8.7 MB/sec    1.00      2.8±0.03ms     9.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
