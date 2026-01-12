```yaml
number: 5130
title: Add Applicability to flake8_quotes fixes
type: pull_request
state: merged
author: evanrittenhouse
labels: []
assignees: []
merged: true
base: main
head: applicability_quotes
created_at: 2023-06-15T19:46:04Z
updated_at: 2023-06-15T21:08:51Z
url: https://github.com/astral-sh/ruff/pull/5130
synced_at: 2026-01-12T15:55:17Z
```

# Add Applicability to flake8_quotes fixes

---

_@evanrittenhouse_

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

_Comment by @github-actions[bot] on 2023-06-15 20:08_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.04      7.2±0.35ms     5.6 MB/sec     1.00      6.9±0.29ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.05  1533.4±72.05µs    10.9 MB/sec     1.00  1464.5±75.04µs    11.4 MB/sec
formatter/numpy/globals.py                 1.00    141.8±7.38µs    20.8 MB/sec     1.05    148.4±8.71µs    19.9 MB/sec
formatter/pydantic/types.py                1.10      3.1±0.21ms     8.2 MB/sec     1.00      2.8±0.13ms     9.0 MB/sec
linter/all-rules/large/dataset.py          1.04     15.5±0.62ms     2.6 MB/sec     1.00     14.9±0.54ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.17ms     4.6 MB/sec     1.01      3.7±0.16ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.06   492.9±33.61µs     6.0 MB/sec     1.00   464.3±37.48µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.29ms     4.0 MB/sec     1.00      6.4±0.33ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.09      7.7±0.36ms     5.3 MB/sec     1.00      7.1±0.27ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.07  1713.7±114.28µs     9.7 MB/sec    1.00  1607.0±63.77µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   191.6±11.58µs    15.4 MB/sec     1.00   191.8±10.52µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.4±0.17ms     7.4 MB/sec     1.00      3.4±0.15ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.03ms     5.2 MB/sec    1.00      7.8±0.04ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1574.3±10.52µs    10.6 MB/sec    1.01   1583.8±9.96µs    10.5 MB/sec
formatter/numpy/globals.py                 1.00    155.6±1.40µs    19.0 MB/sec    1.01    157.3±3.22µs    18.8 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.04ms     8.0 MB/sec    1.00      3.2±0.03ms     8.0 MB/sec
linter/all-rules/large/dataset.py          1.00     16.4±0.08ms     2.5 MB/sec    1.00     16.4±0.09ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.02ms     3.9 MB/sec    1.00      4.2±0.03ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    434.7±5.64µs     6.8 MB/sec    1.00    434.7±6.08µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.04ms     3.5 MB/sec    1.00      7.2±0.08ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.03ms     4.8 MB/sec    1.00      8.4±0.04ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1747.4±15.96µs     9.5 MB/sec    1.00   1745.7±9.96µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.3±1.30µs    15.6 MB/sec    1.00    188.6±1.65µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.02ms     6.7 MB/sec    1.00      3.8±0.01ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @zanieb on 2023-06-15 20:11_

Similar to https://github.com/astral-sh/ruff/pull/5127#issuecomment-1593661870 — I think we should avoid marking fixes as manual unless there's justification since this means we will never apply them for the user. Is there a reason these fixes all need to be manual?

---

_Comment by @evanrittenhouse on 2023-06-15 20:29_

Was more thinking about it in terms of what if the fix goes wrong, but that's not right. I'll change it (and how I think about other fixes)

---

_Merged by @charliermarsh on 2023-06-15 20:50_

---

_Closed by @charliermarsh on 2023-06-15 20:50_

---

_Branch deleted on 2023-06-15 21:08_

---
