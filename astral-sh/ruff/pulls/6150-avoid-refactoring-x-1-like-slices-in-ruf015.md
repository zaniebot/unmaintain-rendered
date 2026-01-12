```yaml
number: 6150
title: "Avoid refactoring `x[:1]`-like slices in RUF015"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/slice
created_at: 2023-07-28T13:02:47Z
updated_at: 2023-07-28T13:38:15Z
url: https://github.com/astral-sh/ruff/pull/6150
synced_at: 2026-01-12T15:55:20Z
```

# Avoid refactoring `x[:1]`-like slices in RUF015

---

_@charliermarsh_

## Summary

Right now, `RUF015` will try to rewrite `x[:1]` as `[next(x)]`. This isn't equivalent if `x`, for example, is empty, where slicing like `x[:1]` is forgiving, but `next` raises `StopIteration`. For me this is a little too much of a deviation to be comfortable with, and most of the value in this rule is the `x[0]` to `next(x)` conversion anyway.

Closes https://github.com/astral-sh/ruff/issues/6148.


---

_Label `bug` added by @charliermarsh on 2023-07-28 13:02_

---

_Comment by @github-actions[bot] on 2023-07-28 13:17_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.8±0.36ms     3.8 MB/sec    1.00     10.7±0.34ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.1±0.06ms     7.9 MB/sec    1.00      2.1±0.05ms     8.0 MB/sec
formatter/numpy/globals.py                 1.00   229.4±10.68µs    12.9 MB/sec    1.04    239.5±7.09µs    12.3 MB/sec
formatter/pydantic/types.py                1.01      4.6±0.12ms     5.5 MB/sec    1.00      4.6±0.20ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.01     14.8±1.13ms     2.8 MB/sec    1.00     14.6±0.43ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.7±0.10ms     4.5 MB/sec    1.00      3.6±0.09ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   506.3±20.52µs     5.8 MB/sec    1.00   504.5±11.07µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.29ms     3.8 MB/sec    1.00      6.6±0.31ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.20ms     5.2 MB/sec    1.00      7.9±0.19ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1621.4±35.03µs    10.3 MB/sec    1.00  1621.9±26.99µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.02    187.3±6.99µs    15.8 MB/sec    1.00    184.0±5.51µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.5±0.11ms     7.4 MB/sec    1.00      3.4±0.09ms     7.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03     11.5±0.42ms     3.5 MB/sec    1.00     11.2±0.46ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.06      2.3±0.08ms     7.3 MB/sec    1.00      2.1±0.09ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00   245.3±13.30µs    12.0 MB/sec    1.01   247.9±18.97µs    11.9 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.16ms     5.4 MB/sec    1.01      4.7±0.20ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.3±0.59ms     2.7 MB/sec    1.01     15.5±0.50ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.1±0.17ms     4.0 MB/sec    1.00      4.0±0.12ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   493.6±21.96µs     6.0 MB/sec    1.01   499.9±25.43µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.22ms     3.7 MB/sec    1.03      7.0±0.31ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.03      8.7±0.34ms     4.7 MB/sec    1.00      8.5±0.29ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1771.5±65.88µs     9.4 MB/sec    1.00  1725.4±67.41µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    205.6±9.48µs    14.4 MB/sec    1.01   207.4±13.15µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.14ms     6.7 MB/sec    1.00      3.8±0.16ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb approved on 2023-07-28 13:29_

---

_Merged by @charliermarsh on 2023-07-28 13:38_

---

_Closed by @charliermarsh on 2023-07-28 13:38_

---

_Branch deleted on 2023-07-28 13:38_

---
