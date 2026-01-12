```yaml
number: 5519
title: "Add rule documentation template to `scripts/add_rule.py`"
type: pull_request
state: merged
author: evanrittenhouse
labels:
  - internal
assignees: []
merged: true
base: main
head: main
created_at: 2023-07-05T01:27:43Z
updated_at: 2023-07-05T01:57:32Z
url: https://github.com/astral-sh/ruff/pull/5519
synced_at: 2026-01-12T15:55:18Z
```

# Add rule documentation template to `scripts/add_rule.py`

---

_@evanrittenhouse_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Saves some typing when using `scripts/add_rule.py` to create a new rule.

## Test Plan

<!-- How was it tested? -->


---

_Renamed from "Add rule documentation template to scripts/add_rule.py" to "Add rule documentation template to `scripts/add_rule.py`" by @evanrittenhouse on 2023-07-05 01:27_

---

_Comment by @github-actions[bot] on 2023-07-05 01:37_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.9±0.01ms     5.2 MB/sec    1.01      8.0±0.02ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1751.0±2.82µs     9.5 MB/sec    1.00   1749.8±4.94µs     9.5 MB/sec
formatter/numpy/globals.py                 1.00    194.1±0.29µs    15.2 MB/sec    1.01    195.3±0.29µs    15.1 MB/sec
formatter/pydantic/types.py                1.01      3.8±0.01ms     6.7 MB/sec    1.00      3.8±0.01ms     6.7 MB/sec
linter/all-rules/large/dataset.py          1.01     13.7±0.10ms     3.0 MB/sec    1.00     13.5±0.08ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     5.0 MB/sec    1.00      3.4±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    434.1±1.25µs     6.8 MB/sec    1.00    434.4±1.45µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.03ms     4.2 MB/sec    1.01      6.1±0.04ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.03ms     6.0 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1485.5±3.10µs    11.2 MB/sec    1.00   1483.9±3.02µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.7±0.22µs    17.3 MB/sec    1.01    172.3±0.91µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.4 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.05ms     4.4 MB/sec    1.08     10.0±0.11ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1971.3±13.63µs     8.4 MB/sec    1.06      2.1±0.02ms     8.0 MB/sec
formatter/numpy/globals.py                 1.00    217.7±1.50µs    13.6 MB/sec    1.04    226.5±4.84µs    13.0 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.02ms     5.7 MB/sec    1.07      4.7±0.03ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.10ms     2.6 MB/sec    1.00     15.5±0.11ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.06ms     4.1 MB/sec    1.00      4.1±0.03ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    430.3±6.98µs     6.9 MB/sec    1.00    430.6±5.07µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.04ms     3.7 MB/sec    1.01      7.0±0.06ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.04ms     5.1 MB/sec    1.00      8.1±0.03ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1647.9±7.35µs    10.1 MB/sec    1.01  1661.2±11.92µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.8±0.87µs    16.4 MB/sec    1.00    180.1±1.46µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.0 MB/sec    1.01      3.6±0.06ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-05 01:57_

---

_Closed by @charliermarsh on 2023-07-05 01:57_

---

_Label `internal` added by @charliermarsh on 2023-07-05 01:57_

---
