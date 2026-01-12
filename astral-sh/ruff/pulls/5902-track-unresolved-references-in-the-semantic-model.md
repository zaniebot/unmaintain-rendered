```yaml
number: 5902
title: Track unresolved references in the semantic model
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/unresolved-references
created_at: 2023-07-19T22:09:10Z
updated_at: 2023-07-19T22:43:57Z
url: https://github.com/astral-sh/ruff/pull/5902
synced_at: 2026-01-12T15:55:19Z
```

# Track unresolved references in the semantic model

---

_@charliermarsh_

## Summary

As part of my continued quest to separate semantic model-building from diagnostic emission, this PR moves our unresolved-reference rules to a deferred pass. So, rather than emitting diagnostics as we encounter unresolved references, we now track those unresolved references on the semantic model (just like resolved references), and after traversal, emit the relevant rules for any unresolved references.


---

_@charliermarsh reviewed on 2023-07-19 22:09_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyflakes/rules/imports.rs`:170 on 2023-07-19 22:09_

I decided to drop this behavior. I don't think it's super important, and it's harder to support now (unless we're comfortable including imports that take place in the same scope, but _after_ a reference, like `print(x); from foo import *`).

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:354 on 2023-07-19 22:10_

Don't love that we're pushing the unresolved reference in this method... but it's consistent with how resolved references are treated. 

---

_@charliermarsh reviewed on 2023-07-19 22:10_

---

_Comment by @github-actions[bot] on 2023-07-19 22:19_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.03ms     4.4 MB/sec    1.00      9.3±0.05ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.01   1872.8±2.24µs     8.9 MB/sec    1.00  1862.2±11.31µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    207.6±0.36µs    14.2 MB/sec    1.00    208.5±2.62µs    14.2 MB/sec
formatter/pydantic/types.py                1.01      4.0±0.09ms     6.3 MB/sec    1.00      4.0±0.01ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     12.9±0.05ms     3.2 MB/sec    1.01     12.9±0.07ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.00ms     5.1 MB/sec    1.00      3.3±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    429.7±2.06µs     6.9 MB/sec    1.00    430.7±0.73µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.02ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.01ms     6.2 MB/sec    1.01      6.6±0.02ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1399.7±2.73µs    11.9 MB/sec    1.02   1424.0±5.67µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.01    156.1±0.24µs    18.9 MB/sec    1.00    155.3±0.26µs    19.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.6 MB/sec    1.01      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.0±0.19ms     3.7 MB/sec    1.10     12.2±0.08ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.03ms     7.7 MB/sec    1.09      2.4±0.04ms     7.0 MB/sec
formatter/numpy/globals.py                 1.00    242.9±3.41µs    12.1 MB/sec    1.06    258.4±4.16µs    11.4 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.09ms     5.4 MB/sec    1.09      5.2±0.06ms     4.9 MB/sec
linter/all-rules/large/dataset.py          1.00     15.1±0.12ms     2.7 MB/sec    1.01     15.2±0.23ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.04ms     4.2 MB/sec    1.00      4.0±0.05ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    487.5±5.47µs     6.1 MB/sec    1.00   485.4±10.32µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.9±0.08ms     3.7 MB/sec    1.00      6.8±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.0±0.08ms     5.1 MB/sec    1.00      7.9±0.05ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1660.8±16.19µs    10.0 MB/sec    1.00  1646.5±29.36µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    190.1±2.54µs    15.5 MB/sec    1.00    188.1±4.71µs    15.7 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.6±0.07ms     7.1 MB/sec    1.00      3.5±0.03ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-19 22:19_

---

_Closed by @charliermarsh on 2023-07-19 22:19_

---

_Branch deleted on 2023-07-19 22:19_

---
