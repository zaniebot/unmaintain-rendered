```yaml
number: 5547
title: "Remove Directive's dependency on Locator"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/noqa-offset
created_at: 2023-07-05T23:27:49Z
updated_at: 2023-07-06T00:01:41Z
url: https://github.com/astral-sh/ruff/pull/5547
synced_at: 2026-01-12T03:36:55Z
```

# Remove Directive's dependency on Locator

---

_Pull request opened by @charliermarsh on 2023-07-05 23:27_

## Summary

It's a bit simpler to let the API just take the text itself, plus an offset (to make the returned `TextRange` absolute, rather than relative).


---

_Label `internal` added by @charliermarsh on 2023-07-05 23:28_

---

_Merged by @charliermarsh on 2023-07-05 23:33_

---

_Closed by @charliermarsh on 2023-07-05 23:33_

---

_Branch deleted on 2023-07-05 23:33_

---

_Comment by @github-actions[bot] on 2023-07-05 23:37_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.02ms     5.2 MB/sec    1.00      7.8±0.02ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1750.4±1.43µs     9.5 MB/sec    1.00   1746.8±3.07µs     9.5 MB/sec
formatter/numpy/globals.py                 1.00    195.9±0.37µs    15.1 MB/sec    1.00    196.7±0.21µs    15.0 MB/sec
formatter/pydantic/types.py                1.01      3.8±0.03ms     6.7 MB/sec    1.00      3.8±0.01ms     6.8 MB/sec
linter/all-rules/large/dataset.py          1.00     13.3±0.02ms     3.1 MB/sec    1.00     13.2±0.03ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    428.7±0.63µs     6.9 MB/sec    1.00    423.9±0.71µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.3 MB/sec    1.00      5.9±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.01ms     6.1 MB/sec    1.00      6.7±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1441.7±8.09µs    11.5 MB/sec    1.01   1454.9±2.32µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.2±0.92µs    18.0 MB/sec    1.00    163.7±0.26µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.01      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.19     12.8±0.49ms     3.2 MB/sec    1.00     10.8±0.54ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.17      2.7±0.17ms     6.2 MB/sec    1.00      2.3±0.18ms     7.2 MB/sec
formatter/numpy/globals.py                 1.05   285.2±17.76µs    10.3 MB/sec    1.00   272.5±27.38µs    10.8 MB/sec
formatter/pydantic/types.py                1.10      5.9±0.34ms     4.3 MB/sec    1.00      5.4±0.25ms     4.7 MB/sec
linter/all-rules/large/dataset.py          1.00     18.0±0.61ms     2.3 MB/sec    1.01     18.3±0.98ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.26ms     3.4 MB/sec    1.01      4.9±0.29ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   575.0±25.41µs     5.1 MB/sec    1.00   577.5±28.18µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.38ms     3.2 MB/sec    1.08      8.6±0.48ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.03      9.5±0.48ms     4.3 MB/sec    1.00      9.3±0.36ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.13ms     8.2 MB/sec    1.00      2.0±0.10ms     8.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   237.8±13.08µs    12.4 MB/sec    1.05   249.1±16.99µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.2±0.24ms     6.0 MB/sec    1.02      4.3±0.25ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
