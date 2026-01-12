```yaml
number: 5607
title: Fix typos found by codespell
type: pull_request
state: merged
author: DimitriPapadopoulos
labels: []
assignees: []
merged: true
base: main
head: codespell
created_at: 2023-07-08T09:02:47Z
updated_at: 2023-07-08T10:35:05Z
url: https://github.com/astral-sh/ruff/pull/5607
synced_at: 2026-01-12T03:36:55Z
```

# Fix typos found by codespell

---

_Pull request opened by @DimitriPapadopoulos on 2023-07-08 09:02_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fix typos found by [codespell](https://github.com/codespell-project/codespell).

I have left out `memoize` for now (see #5606).
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

CI tests.
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-07-08 09:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.1±0.12ms     4.5 MB/sec    1.00      9.1±0.11ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.0±0.03ms     8.2 MB/sec    1.00      2.0±0.02ms     8.2 MB/sec
formatter/numpy/globals.py                 1.00    228.4±4.53µs    12.9 MB/sec    1.00    227.6±4.79µs    13.0 MB/sec
formatter/pydantic/types.py                1.02      4.5±0.05ms     5.7 MB/sec    1.00      4.4±0.06ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.02     15.9±0.18ms     2.6 MB/sec    1.00     15.6±0.15ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.05ms     4.2 MB/sec    1.00      4.0±0.04ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    507.4±7.03µs     5.8 MB/sec    1.00    504.8±6.85µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.06ms     3.6 MB/sec    1.00      7.0±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.02      8.0±0.07ms     5.1 MB/sec    1.00      7.8±0.08ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1744.7±16.25µs     9.5 MB/sec    1.00  1721.6±17.87µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.02    199.0±2.31µs    14.8 MB/sec    1.00    194.8±2.62µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.6±0.03ms     7.0 MB/sec    1.00      3.5±0.05ms     7.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.2±0.44ms     3.6 MB/sec    1.03     11.6±0.54ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.4±0.10ms     7.1 MB/sec    1.05      2.5±0.20ms     6.7 MB/sec
formatter/numpy/globals.py                 1.05   295.4±19.12µs    10.0 MB/sec    1.00   281.8±22.20µs    10.5 MB/sec
formatter/pydantic/types.py                1.00      5.3±0.44ms     4.8 MB/sec    1.09      5.8±0.41ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.00     19.9±1.15ms     2.0 MB/sec    1.03     20.4±1.23ms  2037.3 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      5.2±0.22ms     3.2 MB/sec    1.00      5.0±0.28ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   579.4±32.06µs     5.1 MB/sec    1.05   608.1±24.59µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.06      8.8±0.42ms     2.9 MB/sec    1.00      8.3±0.34ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.6±0.53ms     4.2 MB/sec    1.01      9.7±0.36ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.07ms     8.3 MB/sec    1.04      2.1±0.11ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   253.3±13.39µs    11.6 MB/sec    1.04   264.5±23.47µs    11.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.26ms     5.8 MB/sec    1.04      4.6±0.19ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @DimitriPapadopoulos on 2023-07-08 09:37_

---

_@konstin approved on 2023-07-08 10:33_

---

_Merged by @konstin on 2023-07-08 10:33_

---

_Closed by @konstin on 2023-07-08 10:33_

---

_Branch deleted on 2023-07-08 10:35_

---
