```yaml
number: 6618
title: Update Black tests
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/black
created_at: 2023-08-16T14:55:29Z
updated_at: 2023-08-16T15:20:44Z
url: https://github.com/astral-sh/ruff/pull/6618
synced_at: 2026-01-12T15:55:22Z
```

# Update Black tests

---

_@charliermarsh_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Pulls in some tests that we previously couldn't support

## Test Plan

`cargo test`


---

_@charliermarsh reviewed on 2023-08-16 14:55_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@py_38__pep_572_remove_parens.py.snap`:89 on 2023-08-16 14:55_

Black plans to change this, our formatting is correct.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-16 14:56_

---

_Label `formatter` added by @charliermarsh on 2023-08-16 14:56_

---

_@MichaReiser approved on 2023-08-16 14:56_

---

_Merged by @charliermarsh on 2023-08-16 15:05_

---

_Closed by @charliermarsh on 2023-08-16 15:05_

---

_Branch deleted on 2023-08-16 15:05_

---

_Comment by @github-actions[bot] on 2023-08-16 15:20_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      3.5±0.02ms    11.7 MB/sec    1.00      3.4±0.02ms    11.8 MB/sec
formatter/numpy/ctypeslib.py               1.00    697.6±8.41µs    23.9 MB/sec    1.01   703.7±15.92µs    23.7 MB/sec
formatter/numpy/globals.py                 1.01     73.3±1.01µs    40.3 MB/sec    1.00     72.7±0.64µs    40.6 MB/sec
formatter/pydantic/types.py                1.01  1408.2±32.81µs    18.1 MB/sec    1.00  1392.7±23.66µs    18.3 MB/sec
linter/all-rules/large/dataset.py          1.00     10.6±0.03ms     3.8 MB/sec    1.00     10.6±0.03ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      2.9±0.01ms     5.7 MB/sec    1.00      2.9±0.01ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    325.0±1.89µs     9.1 MB/sec    1.00    324.9±1.37µs     9.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.5±0.01ms     4.6 MB/sec    1.00      5.5±0.01ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.01      5.7±0.01ms     7.2 MB/sec    1.00      5.6±0.01ms     7.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1189.4±3.97µs    14.0 MB/sec    1.00   1187.0±3.12µs    14.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    124.9±0.38µs    23.6 MB/sec    1.00    124.6±1.23µs    23.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.5±0.01ms    10.1 MB/sec    1.00      2.5±0.02ms    10.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.2±0.06ms     9.7 MB/sec    1.00      4.2±0.07ms     9.7 MB/sec
formatter/numpy/ctypeslib.py               1.00   823.0±13.95µs    20.2 MB/sec    1.02   837.8±32.22µs    19.9 MB/sec
formatter/numpy/globals.py                 1.00     84.3±2.50µs    35.0 MB/sec    1.01     85.5±2.51µs    34.5 MB/sec
formatter/pydantic/types.py                1.00  1671.5±21.54µs    15.3 MB/sec    1.01  1692.9±40.75µs    15.1 MB/sec
linter/all-rules/large/dataset.py          1.00     12.7±0.15ms     3.2 MB/sec    1.00     12.8±0.15ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.03ms     4.8 MB/sec    1.00      3.5±0.03ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    437.3±4.96µs     6.7 MB/sec    1.01    439.9±6.76µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.08ms     3.8 MB/sec    1.00      6.7±0.08ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.07ms     5.8 MB/sec    1.00      7.0±0.06ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1481.2±34.89µs    11.2 MB/sec    1.01  1493.5±22.23µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    172.4±4.08µs    17.1 MB/sec    1.00    173.2±3.10µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.04ms     8.1 MB/sec    1.01      3.2±0.04ms     8.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
