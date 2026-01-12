```yaml
number: 4807
title: Fix flake8-fixme architecture
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: fix-flake8-fixme-architecture
created_at: 2023-06-02T11:47:30Z
updated_at: 2023-06-02T16:01:23Z
url: https://github.com/astral-sh/ruff/pull/4807
synced_at: 2026-01-12T15:55:16Z
```

# Fix flake8-fixme architecture

---

_@JonathanPlasse_

- #3747 depends on this to work.
<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

`flake8-fixme` use the old linter architecture. This breaks #3747.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

- The CI job of #3747 works with these changes.
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-06-02 12:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.1±0.13ms     2.9 MB/sec    1.00     14.1±0.16ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    424.3±0.85µs     7.0 MB/sec    1.00    422.0±0.79µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.01ms     6.0 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1501.3±5.89µs    11.1 MB/sec    1.00   1497.7±3.62µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.2±0.32µs    17.3 MB/sec    1.01    171.2±1.30µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.02      5.3±0.00ms     7.7 MB/sec    1.00      5.2±0.01ms     7.8 MB/sec
parser/numpy/ctypeslib.py                  1.01   1025.3±1.50µs    16.2 MB/sec    1.00   1014.6±0.94µs    16.4 MB/sec
parser/numpy/globals.py                    1.01    106.1±0.12µs    27.8 MB/sec    1.00    104.7±0.53µs    28.2 MB/sec
parser/pydantic/types.py                   1.01      2.2±0.00ms    11.3 MB/sec    1.00      2.2±0.01ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.5±0.25ms     2.3 MB/sec    1.00     17.6±0.29ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.09ms     3.7 MB/sec    1.00      4.5±0.10ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    453.1±9.30µs     6.5 MB/sec    1.01    457.3±9.56µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.21ms     3.4 MB/sec    1.00      7.5±0.15ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.12ms     4.7 MB/sec    1.01      8.8±0.15ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1830.7±21.32µs     9.1 MB/sec    1.01  1852.9±34.38µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.0±4.04µs    14.8 MB/sec    1.01    201.8±5.98µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.04ms     6.5 MB/sec    1.02      4.0±0.06ms     6.4 MB/sec
parser/large/dataset.py                    1.00      6.7±0.04ms     6.1 MB/sec    1.00      6.7±0.04ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.01  1271.8±15.92µs    13.1 MB/sec    1.00  1262.7±22.14µs    13.2 MB/sec
parser/numpy/globals.py                    1.01    132.3±1.34µs    22.3 MB/sec    1.00    131.5±1.60µs    22.4 MB/sec
parser/pydantic/types.py                   1.01      2.8±0.03ms     9.0 MB/sec    1.00      2.8±0.02ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-02 13:15_

---

_Closed by @charliermarsh on 2023-06-02 13:15_

---

_Branch deleted on 2023-06-02 16:01_

---
