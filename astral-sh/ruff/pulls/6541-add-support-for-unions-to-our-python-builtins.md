```yaml
number: 6541
title: Add support for unions to our Python builtins type system
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/if-exp
created_at: 2023-08-13T20:40:24Z
updated_at: 2023-08-13T22:11:33Z
url: https://github.com/astral-sh/ruff/pull/6541
synced_at: 2026-01-12T02:52:04Z
```

# Add support for unions to our Python builtins type system

---

_Pull request opened by @charliermarsh on 2023-08-13 20:40_

## Summary

Fixes some TODOs introduced in https://github.com/astral-sh/ruff/pull/6538. In short, given an expression like `1 if x > 0 else "Hello, world!"`, we now return a union type that says the expression can resolve to either an `int` or a `str`. The system remains very limited, it only works for obvious primitive types, and there's no attempt to do inference on any more complex variables. (If any expression yields `Unknown` or `TypeError`, we propagate that result throughout and abort on the client's end.)

---

_Comment by @github-actions[bot] on 2023-08-13 21:10_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02     10.3±0.04ms     3.9 MB/sec    1.00     10.2±0.04ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.03      2.1±0.07ms     8.0 MB/sec    1.00      2.0±0.04ms     8.2 MB/sec
formatter/numpy/globals.py                 1.06    244.1±7.55µs    12.1 MB/sec    1.00    230.2±5.90µs    12.8 MB/sec
formatter/pydantic/types.py                1.02      4.4±0.13ms     5.8 MB/sec    1.00      4.3±0.09ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.0±0.07ms     3.1 MB/sec    1.00     12.9±0.06ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.8 MB/sec    1.00      3.5±0.04ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    494.7±1.18µs     6.0 MB/sec    1.00    495.1±4.47µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.02ms     3.8 MB/sec    1.00      6.7±0.05ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.02ms     6.0 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1500.0±8.76µs    11.1 MB/sec    1.00  1504.1±15.43µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.3±3.82µs    16.5 MB/sec    1.04    186.3±7.14µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.02ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02     10.3±0.14ms     3.9 MB/sec    1.00     10.1±0.05ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1951.0±32.30µs     8.5 MB/sec    1.00  1946.7±42.63µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    197.7±2.73µs    14.9 MB/sec    1.00    198.3±4.37µs    14.9 MB/sec
formatter/pydantic/types.py                1.01      4.3±0.06ms     5.9 MB/sec    1.00      4.3±0.02ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     12.7±0.12ms     3.2 MB/sec    1.01     12.8±0.19ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.7 MB/sec    1.01      3.6±0.03ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    370.0±4.61µs     8.0 MB/sec    1.01    372.7±5.27µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.04ms     3.9 MB/sec    1.01      6.7±0.15ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.04ms     5.9 MB/sec    1.00      6.9±0.05ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1444.5±11.40µs    11.5 MB/sec    1.00  1443.8±12.98µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    149.8±2.06µs    19.7 MB/sec    1.01    151.1±3.99µs    19.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.02ms     8.2 MB/sec    1.01      3.1±0.05ms     8.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-08-13 22:00_

---

_Closed by @charliermarsh on 2023-08-13 22:00_

---

_Branch deleted on 2023-08-13 22:00_

---
