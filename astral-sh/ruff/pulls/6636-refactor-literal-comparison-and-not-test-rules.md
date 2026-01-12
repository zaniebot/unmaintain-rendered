```yaml
number: 6636
title: Refactor literal-comparison and not-test rules
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/rules
created_at: 2023-08-17T00:53:47Z
updated_at: 2023-08-17T01:23:22Z
url: https://github.com/astral-sh/ruff/pull/6636
synced_at: 2026-01-12T02:52:04Z
```

# Refactor literal-comparison and not-test rules

---

_Pull request opened by @charliermarsh on 2023-08-17 00:53_

## Summary

No behavior changes, but these need some refactoring to support https://github.com/astral-sh/ruff/pull/6575 (namely, they need to take the `ast::ExprCompare` or similar node instead of the attribute fields), and I don't want to muddy that PR.

## Test Plan

`cargo test`


---

_Label `internal` added by @charliermarsh on 2023-08-17 00:53_

---

_Merged by @charliermarsh on 2023-08-17 01:02_

---

_Closed by @charliermarsh on 2023-08-17 01:02_

---

_Branch deleted on 2023-08-17 01:02_

---

_Comment by @github-actions[bot] on 2023-08-17 01:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      4.2±0.05ms     9.7 MB/sec    1.00      4.2±0.04ms     9.7 MB/sec
formatter/numpy/ctypeslib.py               1.00   866.6±13.23µs    19.2 MB/sec    1.01   874.5±11.59µs    19.0 MB/sec
formatter/numpy/globals.py                 1.00     89.7±0.87µs    32.9 MB/sec    1.01     90.9±1.94µs    32.5 MB/sec
formatter/pydantic/types.py                1.00  1708.5±16.22µs    14.9 MB/sec    1.02  1738.4±31.54µs    14.7 MB/sec
linter/all-rules/large/dataset.py          1.00     12.2±0.07ms     3.3 MB/sec    1.00     12.2±0.06ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.1 MB/sec    1.00      3.3±0.02ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    466.2±2.50µs     6.3 MB/sec    1.00    465.2±2.41µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.4±0.03ms     4.0 MB/sec    1.00      6.3±0.04ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.4±0.06ms     6.3 MB/sec    1.00      6.4±0.05ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1430.4±7.17µs    11.6 MB/sec    1.00   1428.8±9.33µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.01    168.3±1.11µs    17.5 MB/sec    1.00    167.4±1.76µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.01ms     8.7 MB/sec    1.00      2.9±0.02ms     8.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      4.3±0.17ms     9.5 MB/sec    1.00      4.2±0.10ms     9.7 MB/sec
formatter/numpy/ctypeslib.py               1.00   836.8±25.38µs    19.9 MB/sec    1.00   837.4±39.04µs    19.9 MB/sec
formatter/numpy/globals.py                 1.00     85.8±3.36µs    34.4 MB/sec    1.00     85.9±3.86µs    34.3 MB/sec
formatter/pydantic/types.py                1.00  1709.7±73.24µs    14.9 MB/sec    1.01  1720.1±67.27µs    14.8 MB/sec
linter/all-rules/large/dataset.py          1.00     12.6±0.15ms     3.2 MB/sec    1.01     12.8±0.29ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.05ms     4.8 MB/sec    1.01      3.5±0.07ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   434.0±13.41µs     6.8 MB/sec    1.00   434.4±15.03µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.12ms     3.9 MB/sec    1.01      6.5±0.13ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.11ms     5.8 MB/sec    1.01      7.0±0.13ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1473.2±44.37µs    11.3 MB/sec    1.01  1482.9±31.30µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    171.3±5.29µs    17.2 MB/sec    1.01    172.6±3.82µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.06ms     8.3 MB/sec    1.02      3.1±0.11ms     8.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
