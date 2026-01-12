```yaml
number: 5928
title: Move undefined-local into a post-model-building pass
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/local
created_at: 2023-07-20T17:40:33Z
updated_at: 2023-07-20T19:34:23Z
url: https://github.com/astral-sh/ruff/pull/5928
synced_at: 2026-01-12T03:30:22Z
```

# Move undefined-local into a post-model-building pass

---

_Pull request opened by @charliermarsh on 2023-07-20 17:40_

## Summary

Similar to #5852 and a bunch of related PRs -- trying to move rules that rely on point-in-time semantic analysis to _after_ the semantic model building.


---

_@charliermarsh reviewed on 2023-07-20 17:40_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:4387 on 2023-07-20 17:40_

This isn't necessary -- we do this later, in the `RedefinedWhileUnused` rule, and it's actually specific to that rule, not to shadowing.

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:4379 on 2023-07-20 17:41_

Definitions within a class aren't actually shadowed by (e.g.) definitions within a method of that class.

---

_@charliermarsh reviewed on 2023-07-20 17:41_

---

_Label `internal` added by @charliermarsh on 2023-07-20 17:41_

---

_Comment by @github-actions[bot] on 2023-07-20 17:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.05ms     4.4 MB/sec    1.01      9.4±0.04ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1844.1±4.12µs     9.0 MB/sec    1.01   1855.8±3.77µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    204.2±0.96µs    14.4 MB/sec    1.00    204.9±2.52µs    14.4 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.05ms     6.4 MB/sec    1.00      4.0±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     13.1±0.06ms     3.1 MB/sec    1.00     13.1±0.06ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    433.4±1.54µs     6.8 MB/sec    1.01    437.0±0.69µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.04ms     4.3 MB/sec    1.02      6.0±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.03ms     6.2 MB/sec    1.00      6.6±0.03ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1419.2±5.28µs    11.7 MB/sec    1.00   1424.0±1.55µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    156.4±0.23µs    18.9 MB/sec    1.01    157.6±0.61µs    18.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.6 MB/sec    1.01      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     11.1±0.14ms     3.7 MB/sec    1.00     11.1±0.07ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.1±0.02ms     7.8 MB/sec    1.00      2.1±0.03ms     7.8 MB/sec
formatter/numpy/globals.py                 1.01    230.8±2.16µs    12.8 MB/sec    1.00    227.6±7.58µs    13.0 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.03ms     5.4 MB/sec    1.00      4.7±0.06ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.02     15.8±0.13ms     2.6 MB/sec    1.00     15.5±0.13ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.04ms     4.0 MB/sec    1.00      4.1±0.03ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    420.5±6.65µs     7.0 MB/sec    1.00    419.8±6.14µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.1±0.10ms     3.6 MB/sec    1.00      7.1±0.05ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.02      8.3±0.05ms     4.9 MB/sec    1.00      8.1±0.03ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1676.9±19.12µs     9.9 MB/sec    1.00  1646.3±12.73µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    175.1±1.77µs    16.9 MB/sec    1.00    174.8±1.47µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.03ms     6.9 MB/sec    1.00      3.6±0.02ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-20 19:34_

---

_Closed by @charliermarsh on 2023-07-20 19:34_

---

_Branch deleted on 2023-07-20 19:34_

---
