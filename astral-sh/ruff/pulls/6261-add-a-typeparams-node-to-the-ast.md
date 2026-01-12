```yaml
number: 6261
title: "Add a `TypeParams` node to the AST"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/type-parameter
created_at: 2023-08-02T00:39:54Z
updated_at: 2023-08-02T14:28:56Z
url: https://github.com/astral-sh/ruff/pull/6261
synced_at: 2026-01-12T15:55:21Z
```

# Add a `TypeParams` node to the AST

---

_@charliermarsh_

## Summary

Similar to #6259, this PR adds a `TypeParams` node to the AST, to capture the list of type parameters with their surrounding brackets.

If a statement lacks type parameters, the `type_params` field will be `None`.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-02 00:39_

---

_Review requested from @zanieb by @charliermarsh on 2023-08-02 00:39_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/nodes.rs`:3037 on 2023-08-02 01:01_

I will look into boxing in a separate PR (for type parameters and perhaps arguments).

---

_@charliermarsh reviewed on 2023-08-02 01:01_

---

_Comment by @github-actions[bot] on 2023-08-02 01:03_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.9±0.13ms     4.1 MB/sec    1.00      9.8±0.08ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1937.8±12.57µs     8.6 MB/sec    1.00  1938.9±17.16µs     8.6 MB/sec
formatter/numpy/globals.py                 1.04    226.4±8.31µs    13.0 MB/sec    1.00    218.3±4.40µs    13.5 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.02ms     6.2 MB/sec    1.00      4.1±0.08ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.3±0.06ms     3.1 MB/sec    1.00     13.3±0.10ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    453.8±0.77µs     6.5 MB/sec    1.00    453.2±0.83µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.04ms     4.3 MB/sec    1.00      5.9±0.05ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.01      6.9±0.03ms     5.9 MB/sec    1.00      6.9±0.02ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1440.3±5.30µs    11.6 MB/sec    1.00  1437.0±13.49µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    161.8±5.79µs    18.2 MB/sec    1.00    160.9±4.16µs    18.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.02ms     8.4 MB/sec    1.00      3.0±0.05ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.8±0.38ms     3.8 MB/sec    1.03     11.1±0.50ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.03      2.2±0.10ms     7.7 MB/sec    1.00      2.1±0.11ms     7.9 MB/sec
formatter/numpy/globals.py                 1.02   244.4±28.79µs    12.1 MB/sec    1.00   240.5±17.99µs    12.3 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.20ms     5.4 MB/sec    1.01      4.8±0.23ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.03     15.8±0.60ms     2.6 MB/sec    1.00     15.4±0.88ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.2±0.12ms     4.0 MB/sec    1.00      4.1±0.13ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.07   519.2±20.09µs     5.7 MB/sec    1.00   485.4±25.53µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.04      7.1±0.14ms     3.6 MB/sec    1.00      6.9±0.48ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.04      8.4±0.31ms     4.8 MB/sec    1.00      8.1±0.28ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1738.1±45.87µs     9.6 MB/sec    1.00  1675.0±56.84µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   189.4±10.12µs    15.6 MB/sec    1.00   189.6±10.65µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.7±0.10ms     7.0 MB/sec    1.00      3.6±0.16ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb approved on 2023-08-02 03:30_

Thanks!

---

_@konstin approved on 2023-08-02 07:51_

---

_Merged by @charliermarsh on 2023-08-02 14:12_

---

_Closed by @charliermarsh on 2023-08-02 14:12_

---

_Branch deleted on 2023-08-02 14:12_

---
