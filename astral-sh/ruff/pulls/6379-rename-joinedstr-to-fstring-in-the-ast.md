```yaml
number: 6379
title: "Rename `JoinedStr` to `FString` in the AST"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/f-string
created_at: 2023-08-07T00:55:15Z
updated_at: 2023-08-07T17:49:36Z
url: https://github.com/astral-sh/ruff/pull/6379
synced_at: 2026-01-12T15:55:21Z
```

# Rename `JoinedStr` to `FString` in the AST

---

_@charliermarsh_

## Summary

Per the proposal in https://github.com/astral-sh/ruff/discussions/6183, this PR renames the `JoinedStr` node to `FString`.


---

_Label `internal` added by @charliermarsh on 2023-08-07 00:55_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-07 00:55_

---

_Comment by @github-actions[bot] on 2023-08-07 01:29_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.36ms     4.1 MB/sec    1.03     10.2±0.47ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1935.4±84.22µs     8.6 MB/sec    1.02  1980.4±106.79µs     8.4 MB/sec
formatter/numpy/globals.py                 1.03   227.7±28.85µs    13.0 MB/sec    1.00    220.2±9.34µs    13.4 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.15ms     6.3 MB/sec    1.03      4.2±0.14ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.1±0.60ms     3.1 MB/sec    1.00     13.0±0.42ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      3.6±0.48ms     4.7 MB/sec    1.00      3.4±0.18ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   484.4±23.92µs     6.1 MB/sec    1.04   504.5±37.99µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.27ms     4.3 MB/sec    1.02      6.1±0.41ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      6.8±0.23ms     6.0 MB/sec    1.00      6.7±0.18ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1388.2±41.19µs    12.0 MB/sec    1.00  1393.6±41.25µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    169.9±7.47µs    17.4 MB/sec    1.02    172.9±7.75µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.11ms     8.7 MB/sec    1.01      3.0±0.12ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.1±0.27ms     4.0 MB/sec    1.01     10.1±0.27ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1923.1±28.71µs     8.7 MB/sec    1.02  1970.9±111.46µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00    210.1±3.90µs    14.0 MB/sec    1.01    211.2±7.48µs    14.0 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.07ms     6.1 MB/sec    1.04      4.4±0.16ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.01     13.1±0.18ms     3.1 MB/sec    1.00     12.9±0.31ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.05ms     4.8 MB/sec    1.00      3.5±0.06ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   429.5±11.04µs     6.9 MB/sec    1.00    431.2±9.12µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.14ms     4.3 MB/sec    1.00      5.9±0.14ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.11ms     6.0 MB/sec    1.02      6.9±0.14ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1422.3±22.83µs    11.7 MB/sec    1.02  1449.2±63.02µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.7±3.70µs    17.9 MB/sec    1.01    166.3±6.04µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.04ms     8.5 MB/sec    1.01      3.0±0.06ms     8.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-08-07 07:52_

---

_@MichaReiser approved on 2023-08-07 07:57_

---

_Merged by @charliermarsh on 2023-08-07 17:33_

---

_Closed by @charliermarsh on 2023-08-07 17:33_

---

_Branch deleted on 2023-08-07 17:33_

---
