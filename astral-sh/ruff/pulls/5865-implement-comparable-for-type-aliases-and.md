```yaml
number: 5865
title: "Implement `Comparable` for type aliases and parameters"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/695-comparable
created_at: 2023-07-18T15:45:33Z
updated_at: 2023-07-18T17:35:00Z
url: https://github.com/astral-sh/ruff/pull/5865
synced_at: 2026-01-12T15:55:19Z
```

# Implement `Comparable` for type aliases and parameters

---

_@zanieb_

Part of https://github.com/astral-sh/ruff/issues/5062

---

_Review comment by @konstin on `crates/ruff_python_ast/src/comparable.rs`:991 on 2023-07-18 15:52_

nit: i would ignore range explicitly in all three match arms (`range: _`), so the compiler leads you here if a field gets added to this node.
```suggestion
            ast::TypeParam::TypeVar(ast::TypeParamTypeVar { name, bound, range: _}) => {
```

---

_Comment by @github-actions[bot] on 2023-07-18 15:54_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04     10.2±0.04ms     4.0 MB/sec    1.00      9.9±0.08ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.04   1969.4±4.00µs     8.5 MB/sec    1.00  1901.9±19.20µs     8.8 MB/sec
formatter/numpy/globals.py                 1.02    211.5±0.56µs    13.9 MB/sec    1.00    207.8±0.61µs    14.2 MB/sec
formatter/pydantic/types.py                1.03      4.3±0.01ms     5.9 MB/sec    1.00      4.2±0.02ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.06ms     3.0 MB/sec    1.03     14.0±0.09ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.00ms     4.8 MB/sec    1.02      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    375.9±0.97µs     7.8 MB/sec    1.01    379.4±4.91µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.02ms     4.2 MB/sec    1.03      6.3±0.06ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.04ms     5.8 MB/sec    1.02      7.2±0.06ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1437.4±2.94µs    11.6 MB/sec    1.01   1448.1±4.98µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    148.8±1.23µs    19.8 MB/sec    1.02    151.3±0.30µs    19.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.1 MB/sec    1.01      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.1±0.09ms     3.7 MB/sec    1.00     11.0±0.08ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.1±0.04ms     7.8 MB/sec    1.00      2.1±0.02ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    231.6±3.41µs    12.7 MB/sec    1.00    231.8±3.75µs    12.7 MB/sec
formatter/pydantic/types.py                1.01      4.8±0.04ms     5.4 MB/sec    1.00      4.7±0.03ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.2±0.09ms     2.7 MB/sec    1.03     15.7±0.13ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.1 MB/sec    1.02      4.2±0.11ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   423.6±13.32µs     7.0 MB/sec    1.00    422.7±7.52µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.08ms     3.6 MB/sec    1.00      7.0±0.04ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.05ms     5.0 MB/sec    1.00      8.1±0.04ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1649.4±16.91µs    10.1 MB/sec    1.00  1645.8±20.72µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    175.1±2.26µs    16.9 MB/sec    1.00    173.8±1.73µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.0 MB/sec    1.00      3.6±0.03ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-07-18 15:54_

---

_@charliermarsh approved on 2023-07-18 15:54_

LGTM!

---

_@MichaReiser approved on 2023-07-18 15:54_

---

_Merged by @zanieb on 2023-07-18 17:18_

---

_Closed by @zanieb on 2023-07-18 17:18_

---

_Branch deleted on 2023-07-18 17:18_

---
