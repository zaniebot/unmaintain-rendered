```yaml
number: 5174
title: Fix allowed-ellipsis detection
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ellipsis
created_at: 2023-06-19T04:08:48Z
updated_at: 2023-06-19T04:38:47Z
url: https://github.com/astral-sh/ruff/pull/5174
synced_at: 2026-01-12T03:43:30Z
```

# Fix allowed-ellipsis detection

---

_Pull request opened by @charliermarsh on 2023-06-19 04:08_

## Summary

We weren't resetting the `allow_ellipsis` flag properly, which ultimately caused us to treat the semicolon as "unnecessary" rather than "creating a multi-statement line".

Closes #5154.


---

_Label `bug` added by @charliermarsh on 2023-06-19 04:08_

---

_Merged by @charliermarsh on 2023-06-19 04:19_

---

_Closed by @charliermarsh on 2023-06-19 04:19_

---

_Branch deleted on 2023-06-19 04:19_

---

_Comment by @github-actions[bot] on 2023-06-19 04:23_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04      7.1±0.01ms     5.8 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.03   1430.3±2.01µs    11.6 MB/sec    1.00   1393.1±4.44µs    12.0 MB/sec
formatter/numpy/globals.py                 1.03    141.2±0.45µs    20.9 MB/sec    1.00    136.4±0.82µs    21.6 MB/sec
formatter/pydantic/types.py                1.04      2.9±0.01ms     8.9 MB/sec    1.00      2.8±0.01ms     9.2 MB/sec
linter/all-rules/large/dataset.py          1.01     14.5±0.05ms     2.8 MB/sec    1.00     14.4±0.05ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.01ms     4.7 MB/sec    1.00      3.5±0.00ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    371.9±1.97µs     7.9 MB/sec    1.00    368.0±2.66µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.02ms     4.1 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.02ms     5.5 MB/sec    1.00      7.4±0.02ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1539.8±3.63µs    10.8 MB/sec    1.00   1533.4±2.46µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    165.4±0.21µs    17.8 MB/sec    1.00    163.8±0.69µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      8.0±0.27ms     5.1 MB/sec    1.00      7.8±0.06ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1594.7±29.97µs    10.4 MB/sec    1.01  1610.9±72.88µs    10.3 MB/sec
formatter/numpy/globals.py                 1.00    155.0±1.37µs    19.0 MB/sec    1.02    157.8±4.06µs    18.7 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.09ms     7.9 MB/sec    1.00      3.2±0.06ms     7.9 MB/sec
linter/all-rules/large/dataset.py          1.00     15.9±0.22ms     2.6 MB/sec    1.01     16.0±0.35ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.03ms     4.0 MB/sec    1.01      4.2±0.12ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   427.2±10.99µs     6.9 MB/sec    1.01    430.4±6.59µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.17ms     3.6 MB/sec    1.00      7.0±0.18ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.08ms     4.9 MB/sec    1.00      8.4±0.13ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1785.1±65.36µs     9.3 MB/sec    1.00  1771.6±83.43µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    190.5±7.12µs    15.5 MB/sec    1.00    189.6±4.08µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.8±0.12ms     6.6 MB/sec    1.00      3.8±0.05ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
