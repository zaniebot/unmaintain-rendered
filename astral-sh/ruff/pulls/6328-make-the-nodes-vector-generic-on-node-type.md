```yaml
number: 6328
title: "Make the `Nodes` vector generic on node type"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/expressions
created_at: 2023-08-04T03:47:41Z
updated_at: 2023-08-04T13:18:42Z
url: https://github.com/astral-sh/ruff/pull/6328
synced_at: 2026-01-12T02:52:04Z
```

# Make the `Nodes` vector generic on node type

---

_Pull request opened by @charliermarsh on 2023-08-04 03:47_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-08-04 03:47_

---

_Merged by @charliermarsh on 2023-08-04 03:57_

---

_Closed by @charliermarsh on 2023-08-04 03:57_

---

_Branch deleted on 2023-08-04 03:57_

---

_Comment by @github-actions[bot] on 2023-08-04 04:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.0±0.05ms     5.1 MB/sec    1.00      8.0±0.03ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.01  1586.6±18.34µs    10.5 MB/sec    1.00  1574.4±11.92µs    10.6 MB/sec
formatter/numpy/globals.py                 1.00    168.7±0.31µs    17.5 MB/sec    1.00    168.0±0.95µs    17.6 MB/sec
formatter/pydantic/types.py                1.01      3.5±0.03ms     7.4 MB/sec    1.00      3.4±0.02ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.04     11.8±0.14ms     3.5 MB/sec    1.00     11.3±0.13ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      2.9±0.01ms     5.7 MB/sec    1.00      2.9±0.01ms     5.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    317.4±1.38µs     9.3 MB/sec    1.00    316.0±0.76µs     9.3 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.3±0.05ms     4.9 MB/sec    1.00      5.2±0.05ms     4.9 MB/sec
linter/default-rules/large/dataset.py      1.03      5.6±0.06ms     7.3 MB/sec    1.00      5.4±0.03ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1130.9±3.84µs    14.7 MB/sec    1.00   1112.1±4.03µs    15.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    115.9±0.77µs    25.5 MB/sec    1.00    114.6±0.34µs    25.7 MB/sec
linter/default-rules/pydantic/types.py     1.03      2.5±0.01ms    10.3 MB/sec    1.00      2.4±0.01ms    10.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.6±0.09ms     4.2 MB/sec    1.01      9.7±0.09ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1867.0±26.35µs     8.9 MB/sec    1.01  1884.9±19.62µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    207.6±4.32µs    14.2 MB/sec    1.01    210.6±9.76µs    14.0 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.05ms     6.2 MB/sec    1.01      4.1±0.06ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.4±0.14ms     3.0 MB/sec    1.01     13.6±0.15ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.04ms     4.8 MB/sec    1.01      3.5±0.03ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    429.0±7.18µs     6.9 MB/sec    1.01   432.5±12.10µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.09ms     4.2 MB/sec    1.01      6.2±0.07ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.06ms     6.0 MB/sec    1.01      6.8±0.06ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1396.8±14.71µs    11.9 MB/sec    1.01  1411.3±15.67µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    157.7±3.05µs    18.7 MB/sec    1.01    159.5±6.38µs    18.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.06ms     8.4 MB/sec    1.01      3.1±0.03ms     8.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/node.rs`:21 on 2023-08-04 05:43_

What do you think about using `AnyNodeRef` here?

---

_@MichaReiser reviewed on 2023-08-04 05:43_

---

_@konstin approved on 2023-08-04 06:00_

---

_@charliermarsh reviewed on 2023-08-04 13:18_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/node.rs`:21 on 2023-08-04 13:18_

Hm, but I want the struct to be generic on nodes of a specific type (e.g., an `IndexVec` for statements, and one for expressions, rather than an `IndexVec` that can hold a mixture of nodes). Is that achievable with `AnyNodeRef`?

---
