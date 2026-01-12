```yaml
number: 6304
title: "Update `UP032` to support `await` expressions"
type: pull_request
state: merged
author: harupy
labels:
  - rule
assignees: []
merged: true
base: main
head: UP032-await
created_at: 2023-08-03T14:39:22Z
updated_at: 2023-08-03T15:16:54Z
url: https://github.com/astral-sh/ruff/pull/6304
synced_at: 2026-01-12T02:52:04Z
```

# Update `UP032` to support `await` expressions

---

_Pull request opened by @harupy on 2023-08-03 14:39_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

In Python >= 3.7, `await` can be included in f-strings. 

https://bugs.python.org/issue28942

## Test Plan

<!-- How was it tested? -->

Existing tests


---

_@zanieb approved on 2023-08-03 14:50_

Huh, I didn't know that.

---

_Label `rule` added by @zanieb on 2023-08-03 14:52_

---

_Comment by @github-actions[bot] on 2023-08-03 14:52_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.5±0.08ms     4.8 MB/sec    1.00      8.5±0.09ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1667.3±35.44µs    10.0 MB/sec    1.02  1694.3±42.21µs     9.8 MB/sec
formatter/numpy/globals.py                 1.01    172.9±5.51µs    17.1 MB/sec    1.00    171.6±2.38µs    17.2 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.07ms     7.0 MB/sec    1.01      3.7±0.09ms     7.0 MB/sec
linter/all-rules/large/dataset.py          1.01     11.3±0.06ms     3.6 MB/sec    1.00     11.2±0.04ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.02ms     5.7 MB/sec    1.00      2.9±0.03ms     5.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    317.9±1.19µs     9.3 MB/sec    1.00    318.9±1.35µs     9.3 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.1±0.01ms     5.0 MB/sec    1.00      5.1±0.01ms     5.0 MB/sec
linter/default-rules/large/dataset.py      1.01      6.0±0.03ms     6.8 MB/sec    1.00      5.9±0.01ms     6.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1202.5±5.96µs    13.8 MB/sec    1.00   1193.6±9.14µs    14.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    120.3±2.61µs    24.5 MB/sec    1.00    119.0±0.36µs    24.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.6±0.01ms     9.7 MB/sec    1.00      2.6±0.01ms     9.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.22     15.5±0.56ms     2.6 MB/sec    1.00     12.7±0.37ms     3.2 MB/sec
formatter/numpy/ctypeslib.py               1.13      2.8±0.15ms     6.0 MB/sec    1.00      2.4±0.08ms     6.8 MB/sec
formatter/numpy/globals.py                 1.11   303.9±28.95µs     9.7 MB/sec    1.00   275.0±16.70µs    10.7 MB/sec
formatter/pydantic/types.py                1.18      6.3±0.23ms     4.1 MB/sec    1.00      5.3±0.15ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.11     21.9±0.60ms  1904.5 KB/sec    1.00     19.7±0.50ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.12      5.7±0.15ms     2.9 MB/sec    1.00      5.1±0.22ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.06   636.0±26.89µs     4.6 MB/sec    1.00   602.3±21.68µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.14      9.8±0.29ms     2.6 MB/sec    1.00      8.6±0.37ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.06     12.0±0.46ms     3.4 MB/sec    1.00     11.3±0.50ms     3.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.09      2.2±0.11ms     7.5 MB/sec    1.00      2.0±0.08ms     8.1 MB/sec
linter/default-rules/numpy/globals.py      1.07    254.8±9.24µs    11.6 MB/sec    1.00   238.0±22.92µs    12.4 MB/sec
linter/default-rules/pydantic/types.py     1.19      5.1±0.39ms     5.0 MB/sec    1.00      4.3±0.27ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @zanieb on 2023-08-03 14:53_

---

_Closed by @zanieb on 2023-08-03 14:53_

---

_Branch deleted on 2023-08-03 14:54_

---
