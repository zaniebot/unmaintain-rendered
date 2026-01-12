```yaml
number: 4795
title: Move unused imports rule into its own module
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/unused-imports
created_at: 2023-06-02T04:20:12Z
updated_at: 2023-06-02T04:54:05Z
url: https://github.com/astral-sh/ruff/pull/4795
synced_at: 2026-01-12T15:55:16Z
```

# Move unused imports rule into its own module

---

_@charliermarsh_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

I think this was actually _impossible_ to do while we required deletions to be stored on `Checker` due to lifetimes and mutability issues.

No improvements to the rule itself, or even the logic within -- just getting it out of `Checker`.


---

_Label `internal` added by @charliermarsh on 2023-06-02 04:20_

---

_Merged by @charliermarsh on 2023-06-02 04:27_

---

_Closed by @charliermarsh on 2023-06-02 04:27_

---

_Branch deleted on 2023-06-02 04:27_

---

_Comment by @github-actions[bot] on 2023-06-02 04:31_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.0±0.02ms     2.7 MB/sec    1.01     15.1±0.14ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.7±0.01ms     4.5 MB/sec    1.00      3.6±0.00ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.01    377.6±1.44µs     7.8 MB/sec    1.00    373.0±1.56µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.3±0.02ms     4.1 MB/sec    1.00      6.3±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.5±0.02ms     5.4 MB/sec    1.00      7.4±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1599.5±3.00µs    10.4 MB/sec    1.00   1582.4±5.54µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.03    176.4±0.50µs    16.7 MB/sec    1.00    171.5±0.41µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.4±0.01ms     7.5 MB/sec    1.00      3.4±0.01ms     7.6 MB/sec
parser/large/dataset.py                    1.22      6.9±0.00ms     5.9 MB/sec    1.00      5.7±0.00ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.17   1314.9±0.83µs    12.7 MB/sec    1.00   1122.1±1.58µs    14.8 MB/sec
parser/numpy/globals.py                    1.13    130.6±0.20µs    22.6 MB/sec    1.00    115.5±0.90µs    25.5 MB/sec
parser/pydantic/types.py                   1.18      2.9±0.00ms     8.8 MB/sec    1.00      2.4±0.00ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.9±0.26ms     2.4 MB/sec    1.00     16.9±0.19ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.04ms     3.9 MB/sec    1.01      4.3±0.06ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    503.6±7.25µs     5.9 MB/sec    1.01    509.1±9.20µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.09ms     3.6 MB/sec    1.02      7.2±0.18ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.11ms     4.9 MB/sec    1.01      8.3±0.07ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1757.3±23.93µs     9.5 MB/sec    1.01  1777.0±18.66µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    201.1±3.03µs    14.7 MB/sec    1.02   206.0±10.76µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.9 MB/sec    1.02      3.8±0.06ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.5±0.04ms     6.3 MB/sec    1.00      6.5±0.05ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1220.9±26.95µs    13.6 MB/sec    1.00  1226.5±24.04µs    13.6 MB/sec
parser/numpy/globals.py                    1.01    125.8±2.33µs    23.5 MB/sec    1.00    124.7±1.81µs    23.7 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.3 MB/sec    1.00      2.8±0.03ms     9.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
