```yaml
number: 6052
title: "`pycodestyle.max-doc-length` doc updates"
type: pull_request
state: merged
author: scop
labels:
  - documentation
assignees: []
merged: true
base: main
head: doc/w505-max-doc-length
created_at: 2023-07-25T04:34:48Z
updated_at: 2023-07-25T15:58:26Z
url: https://github.com/astral-sh/ruff/pull/6052
synced_at: 2026-01-12T03:30:22Z
```

# `pycodestyle.max-doc-length` doc updates

---

_Pull request opened by @scop on 2023-07-25 04:34_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

SSIA.

## Test Plan

<!-- How was it tested? -->

N/A.

---

_Comment by @github-actions[bot] on 2023-07-25 04:47_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.07ms     4.3 MB/sec    1.00      9.3±0.04ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1857.5±2.40µs     9.0 MB/sec    1.01  1867.1±14.88µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    208.5±4.11µs    14.2 MB/sec    1.00    207.8±1.22µs    14.2 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.4 MB/sec    1.01      4.0±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.01     12.9±0.09ms     3.1 MB/sec    1.00     12.8±0.06ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.1 MB/sec    1.00      3.2±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    424.1±1.87µs     7.0 MB/sec    1.00    426.2±0.61µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.02      5.9±0.11ms     4.3 MB/sec    1.00      5.8±0.04ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.03      6.8±0.05ms     6.0 MB/sec    1.00      6.6±0.05ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1423.2±3.91µs    11.7 MB/sec    1.00   1416.9±2.51µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.2±0.27µs    18.7 MB/sec    1.01    159.0±0.30µs    18.6 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.0±0.58ms     3.4 MB/sec    1.06     12.7±0.44ms     3.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.3±0.07ms     7.2 MB/sec    1.04      2.4±0.10ms     6.9 MB/sec
formatter/numpy/globals.py                 1.00   256.3±11.40µs    11.5 MB/sec    1.03   263.2±13.02µs    11.2 MB/sec
formatter/pydantic/types.py                1.00      5.1±0.21ms     5.0 MB/sec    1.03      5.3±0.23ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.00     17.7±0.65ms     2.3 MB/sec    1.01     17.8±0.76ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.23ms     3.7 MB/sec    1.03      4.6±0.22ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   522.8±19.32µs     5.6 MB/sec    1.00   521.5±15.36µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.9±0.35ms     3.2 MB/sec    1.01      8.0±0.40ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      9.3±0.48ms     4.4 MB/sec    1.02      9.5±0.36ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1819.2±59.80µs     9.2 MB/sec    1.03  1881.9±96.98µs     8.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    205.2±6.58µs    14.4 MB/sec    1.01    206.6±9.20µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.18ms     6.3 MB/sec    1.00      4.1±0.19ms     6.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Note `pycodestyle.max-doc-length` in W505 docs" to "`pycodestyle.max-doc-length` doc updates" by @scop on 2023-07-25 04:50_

---

_Label `documentation` added by @charliermarsh on 2023-07-25 15:26_

---

_Comment by @charliermarsh on 2023-07-25 15:26_

Thanks!

---

_Merged by @charliermarsh on 2023-07-25 15:34_

---

_Closed by @charliermarsh on 2023-07-25 15:34_

---

_Branch deleted on 2023-07-25 15:51_

---
