```yaml
number: 5083
title: Add a dedicated read result for unbound locals
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/unbound-local
created_at: 2023-06-14T13:36:33Z
updated_at: 2023-06-14T14:11:47Z
url: https://github.com/astral-sh/ruff/pull/5083
synced_at: 2026-01-12T03:43:30Z
```

# Add a dedicated read result for unbound locals

---

_Pull request opened by @charliermarsh on 2023-06-14 13:36_

## Summary

Small follow-up to #4888 to add a dedicated `ResolvedRead` case for unbound locals, mostly for clarity and documentation purposes (no behavior changes).

## Test Plan

`cargo test`


---

_Review requested from @konstin by @charliermarsh on 2023-06-14 13:36_

---

_Label `internal` added by @charliermarsh on 2023-06-14 13:36_

---

_Review comment by @konstin on `crates/ruff_python_semantic/src/model.rs`:1073 on 2023-06-14 13:38_

These are great docs!

---

_@konstin approved on 2023-06-14 13:42_

---

_Comment by @github-actions[bot] on 2023-06-14 13:48_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.9±0.01ms     5.9 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.01   1398.2±3.16µs    11.9 MB/sec    1.00   1383.7±2.22µs    12.0 MB/sec
formatter/numpy/globals.py                 1.00    137.3±0.27µs    21.5 MB/sec    1.00    136.8±0.13µs    21.6 MB/sec
formatter/pydantic/types.py                1.01      2.8±0.01ms     9.1 MB/sec    1.00      2.8±0.03ms     9.3 MB/sec
linter/all-rules/large/dataset.py          1.01     14.8±0.06ms     2.7 MB/sec    1.00     14.7±0.06ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.6 MB/sec    1.01      3.6±0.00ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    371.2±2.13µs     7.9 MB/sec    1.00    372.8±1.53µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.01ms     4.1 MB/sec    1.00      6.2±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.02ms     5.5 MB/sec    1.00      7.4±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1547.3±4.41µs    10.8 MB/sec    1.00   1543.6±4.02µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.1±0.31µs    17.9 MB/sec    1.01    166.7±1.53µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.03ms     7.6 MB/sec    1.00      3.3±0.00ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.09ms     5.2 MB/sec    1.00      7.8±0.06ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1589.8±18.77µs    10.5 MB/sec    1.01  1601.7±26.34µs    10.4 MB/sec
formatter/numpy/globals.py                 1.01   155.8±17.90µs    18.9 MB/sec    1.00    154.5±6.14µs    19.1 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.05ms     8.0 MB/sec    1.01      3.2±0.04ms     8.0 MB/sec
linter/all-rules/large/dataset.py          1.00     17.1±0.36ms     2.4 MB/sec    1.01     17.3±0.36ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.05ms     3.8 MB/sec    1.01      4.4±0.08ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    510.0±8.02µs     5.8 MB/sec    1.00    508.9±6.61µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.09ms     3.4 MB/sec    1.00      7.4±0.13ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.10ms     4.8 MB/sec    1.00      8.4±0.08ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1750.2±15.94µs     9.5 MB/sec    1.02  1790.2±16.55µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    202.6±3.81µs    14.6 MB/sec    1.01    205.4±3.96µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.8 MB/sec    1.00      3.8±0.04ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-14 13:58_

---

_Closed by @charliermarsh on 2023-06-14 13:58_

---

_Branch deleted on 2023-06-14 13:58_

---
