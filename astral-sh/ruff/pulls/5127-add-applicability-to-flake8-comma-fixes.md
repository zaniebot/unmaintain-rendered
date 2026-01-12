```yaml
number: 5127
title: Add Applicability to flake8_comma fixes
type: pull_request
state: merged
author: evanrittenhouse
labels: []
assignees: []
merged: true
base: main
head: applicability
created_at: 2023-06-15T19:40:29Z
updated_at: 2023-06-15T21:08:42Z
url: https://github.com/astral-sh/ruff/pull/5127
synced_at: 2026-01-12T03:43:30Z
```

# Add Applicability to flake8_comma fixes

---

_Pull request opened by @evanrittenhouse on 2023-06-15 19:40_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes some of #4184 

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-06-15 20:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                     pr
-----                                      ----                                     --
formatter/large/dataset.py                 1.09      8.0±3.54ms     5.1 MB/sec      1.00      7.4±0.35ms     5.5 MB/sec
formatter/numpy/ctypeslib.py               1.00  1453.4±300.06µs    11.5 MB/sec     1.07  1549.2±90.42µs    10.7 MB/sec
formatter/numpy/globals.py                 1.00    134.0±9.18µs    22.0 MB/sec      1.10   147.1±13.09µs    20.1 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.15ms     9.6 MB/sec      1.19      3.2±0.22ms     8.1 MB/sec
linter/all-rules/large/dataset.py          1.22     16.9±0.34ms     2.4 MB/sec      1.00     13.9±0.56ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.22      4.1±0.14ms     4.1 MB/sec      1.00      3.4±0.16ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.18   507.1±27.30µs     5.8 MB/sec      1.00   429.9±31.30µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.16      7.1±0.27ms     3.6 MB/sec      1.00      6.1±0.37ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.13      7.7±0.61ms     5.3 MB/sec      1.00      6.8±0.32ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.27  1874.8±1529.19µs     8.9 MB/sec    1.00  1472.6±83.08µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.15   201.1±82.32µs    14.7 MB/sec      1.00   174.3±10.29µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.21      3.7±0.91ms     6.8 MB/sec      1.00      3.1±0.15ms     8.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      7.9±0.19ms     5.2 MB/sec    1.00      7.7±0.18ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1584.8±28.62µs    10.5 MB/sec    1.00  1583.0±31.39µs    10.5 MB/sec
formatter/numpy/globals.py                 1.00    148.9±3.54µs    19.8 MB/sec    1.04    154.1±9.29µs    19.1 MB/sec
formatter/pydantic/types.py                1.01      3.2±0.06ms     7.9 MB/sec    1.00      3.2±0.11ms     7.9 MB/sec
linter/all-rules/large/dataset.py          1.00     16.8±0.37ms     2.4 MB/sec    1.03     17.3±0.93ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.09ms     3.9 MB/sec    1.05      4.4±0.23ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   499.3±16.32µs     5.9 MB/sec    1.04   517.4±50.75µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.4±0.25ms     3.4 MB/sec    1.00      7.3±0.29ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      8.5±0.21ms     4.8 MB/sec    1.00      8.4±0.17ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1777.8±36.01µs     9.4 MB/sec    1.00  1765.6±47.13µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    201.0±3.95µs    14.7 MB/sec    1.03   207.0±11.27µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.08ms     6.7 MB/sec    1.00      3.8±0.08ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @zanieb on 2023-06-15 20:07_

Is there a reason you chose `manual` here? It seems like these fixes could always be applied safely without changing the meaning of the code.

---

_Comment by @evanrittenhouse on 2023-06-15 20:12_

Changed to `automatic` - thanks, I think I got mixed up after going through a bunch in a short time!

---

_Merged by @charliermarsh on 2023-06-15 20:49_

---

_Closed by @charliermarsh on 2023-06-15 20:49_

---

_Branch deleted on 2023-06-15 21:08_

---
