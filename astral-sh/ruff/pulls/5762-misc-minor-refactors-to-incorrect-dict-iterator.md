```yaml
number: 5762
title: "Misc. minor refactors to `incorrect-dict-iterator`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/unused-loop
created_at: 2023-07-14T17:23:39Z
updated_at: 2023-07-14T17:55:05Z
url: https://github.com/astral-sh/ruff/pull/5762
synced_at: 2026-01-12T03:30:21Z
```

# Misc. minor refactors to `incorrect-dict-iterator`

---

_Pull request opened by @charliermarsh on 2023-07-14 17:23_

## Summary

Mostly a no-op: use a single match for key-value, use identifier range rather than re-lexing, respect our `dummy-variable-rgx` setting.

---

_Label `internal` added by @charliermarsh on 2023-07-14 17:23_

---

_@charliermarsh reviewed on 2023-07-14 17:29_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/tryceratops/rules/reraise_no_cause.rs`:58 on 2023-07-14 17:29_

Unrelated, I just find this very confusing (the `case.is_none()` has nothing to do with whether `exc` is `Some`).

---

_Merged by @charliermarsh on 2023-07-14 17:29_

---

_Closed by @charliermarsh on 2023-07-14 17:29_

---

_Branch deleted on 2023-07-14 17:29_

---

_Comment by @github-actions[bot] on 2023-07-14 17:33_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.08     12.3±0.08ms     3.3 MB/sec    1.00     11.4±0.10ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.07      2.4±0.01ms     7.0 MB/sec    1.00      2.2±0.01ms     7.5 MB/sec
formatter/numpy/globals.py                 1.05    261.7±0.69µs    11.3 MB/sec    1.00    248.5±1.19µs    11.9 MB/sec
formatter/pydantic/types.py                1.09      5.2±0.01ms     4.9 MB/sec    1.00      4.8±0.05ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     16.3±0.16ms     2.5 MB/sec    1.00     16.2±0.09ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.1 MB/sec    1.00      4.1±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    533.8±3.62µs     5.5 MB/sec    1.00    528.1±3.83µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.04ms     3.5 MB/sec    1.00      7.2±0.05ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.09ms     5.1 MB/sec    1.01      8.0±0.05ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1726.0±16.28µs     9.6 MB/sec    1.03  1785.5±64.26µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.0±1.12µs    14.8 MB/sec    1.00    199.7±1.76µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.1 MB/sec    1.01      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.2±0.09ms     3.6 MB/sec    1.00     11.2±0.09ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.02ms     7.8 MB/sec    1.00      2.1±0.02ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    233.5±7.24µs    12.6 MB/sec    1.00   233.5±12.00µs    12.6 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.03ms     5.4 MB/sec    1.00      4.7±0.03ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.8±0.13ms     2.6 MB/sec    1.00     15.9±0.14ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.06ms     4.0 MB/sec    1.01      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    429.9±6.19µs     6.9 MB/sec    1.00    427.7±6.83µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.07ms     3.5 MB/sec    1.00      7.2±0.10ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      8.1±0.06ms     5.0 MB/sec    1.00      8.1±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1681.0±18.83µs     9.9 MB/sec    1.00  1654.3±10.14µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.02    182.4±2.56µs    16.2 MB/sec    1.00    179.1±1.88µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     7.0 MB/sec    1.00      3.7±0.04ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
