```yaml
number: 5265
title: Remove visit_arg_with_default
type: pull_request
state: merged
author: jgberry
labels: []
assignees: []
merged: true
base: main
head: fix-ast-visitor-order
created_at: 2023-06-21T18:57:42Z
updated_at: 2023-06-21T20:05:21Z
url: https://github.com/astral-sh/ruff/pull/5265
synced_at: 2026-01-12T03:43:30Z
```

# Remove visit_arg_with_default

---

_Pull request opened by @jgberry on 2023-06-21 18:57_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This is a follow up to #5221. Turns out it was easy to restructure the visitor to get the right order, I'm just dumb :man_shrugging:  I've removed `visit_arg_with_default` entirely from the `Visitor`, although it still exists as part of `preorder::Visitor`.


---

_Comment by @github-actions[bot] on 2023-06-21 19:09_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.17      7.7±0.01ms     5.3 MB/sec    1.00      6.5±0.02ms     6.2 MB/sec
formatter/numpy/ctypeslib.py               1.12   1534.3±2.08µs    10.9 MB/sec    1.00   1367.8±4.88µs    12.2 MB/sec
formatter/numpy/globals.py                 1.09    144.4±0.21µs    20.4 MB/sec    1.00    131.9±0.88µs    22.4 MB/sec
formatter/pydantic/types.py                1.15      3.2±0.00ms     8.1 MB/sec    1.00      2.8±0.00ms     9.3 MB/sec
linter/all-rules/large/dataset.py          1.00     13.1±0.04ms     3.1 MB/sec    1.00     13.0±0.03ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.00ms     4.9 MB/sec    1.00      3.3±0.02ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.02    437.3±6.84µs     6.7 MB/sec    1.00    428.3±0.35µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.9±0.02ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.01ms     6.2 MB/sec    1.01      6.7±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1458.2±2.90µs    11.4 MB/sec    1.02   1481.1±1.57µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.3±0.30µs    18.1 MB/sec    1.03    167.7±0.18µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.01      3.1±0.00ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.02      9.6±0.45ms     4.2 MB/sec     1.00      9.4±0.35ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1928.0±143.34µs     8.6 MB/sec    1.01  1943.4±84.24µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00   188.8±13.14µs    15.6 MB/sec     1.05   198.3±15.03µs    14.9 MB/sec
formatter/pydantic/types.py                1.00      3.9±0.18ms     6.5 MB/sec     1.01      4.0±0.14ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     18.3±0.80ms     2.2 MB/sec     1.03     18.8±0.67ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.21ms     3.5 MB/sec     1.05      5.0±0.22ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   566.1±27.73µs     5.2 MB/sec     1.07   605.6±28.51µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.34ms     3.2 MB/sec     1.08      8.6±0.43ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.7±0.33ms     4.2 MB/sec     1.00      9.7±0.45ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.12ms     8.3 MB/sec     1.04      2.1±0.08ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   230.7±11.06µs    12.8 MB/sec     1.06   244.2±19.49µs    12.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.18ms     6.1 MB/sec     1.10      4.5±0.17ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-21 20:00_

---

_Closed by @charliermarsh on 2023-06-21 20:00_

---

_Comment by @charliermarsh on 2023-06-21 20:00_

LGTM!

---

_Branch deleted on 2023-06-21 20:05_

---
