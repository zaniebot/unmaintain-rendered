```yaml
number: 6253
title: "Rename `Arguments` to `Parameters` in the AST"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/parameters
created_at: 2023-08-01T17:04:47Z
updated_at: 2023-08-01T18:11:49Z
url: https://github.com/astral-sh/ruff/pull/6253
synced_at: 2026-01-12T02:58:30Z
```

# Rename `Arguments` to `Parameters` in the AST

---

_Pull request opened by @charliermarsh on 2023-08-01 17:04_

## Summary

This PR renames a few AST nodes for clarity:

- `Arguments` is now `Parameters`
- `Arg` is now `Parameter`
- `ArgWithDefault` is now `ParameterWithDefault`

For now, the attribute names that reference `Parameters` directly are changed (e.g., on `StmtFunctionDef`), but the attributes on `Parameters` itself are not (e.g., `vararg`). We may revisit that decision in the future.

For context, the AST node formerly known as `Arguments` is used in function definitions. Formally (outside of the Python context), "arguments" typically refers to "the values passed to a function", while "parameters" typically refers to "the variables used in a function definition". E.g., if you Google "arguments vs parameters", you'll get some explanation like:

> A parameter is a variable in a function definition. It is a placeholder and hence does not have a concrete value. An argument is a value passed during function invocation.

We're thus deviating from Python's nomenclature in favor of a scheme that we find to be more precise.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-01 17:04_

---

_Review requested from @zanieb by @charliermarsh on 2023-08-01 17:04_

---

_Review requested from @konstin by @charliermarsh on 2023-08-01 17:04_

---

_@charliermarsh reviewed on 2023-08-01 17:07_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/nodes.rs`:1827 on 2023-08-01 17:07_

I removed this -- I don't think it's used anymore?

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/node.rs`:3666 on 2023-08-01 17:07_

Rename

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/node.rs`:95 on 2023-08-01 17:09_

Rename the variants to match the new node names

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:143 on 2023-08-01 17:10_

Should we rename the field to `parameters`?

---

_@MichaReiser approved on 2023-08-01 17:10_

---

_@charliermarsh reviewed on 2023-08-01 17:12_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/node.rs`:95 on 2023-08-01 17:12_

On it.

---

_@zanieb approved on 2023-08-01 17:40_

I read most of this :)

---

_Merged by @charliermarsh on 2023-08-01 17:53_

---

_Closed by @charliermarsh on 2023-08-01 17:53_

---

_Branch deleted on 2023-08-01 17:53_

---

_Comment by @github-actions[bot] on 2023-08-01 17:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.2±0.07ms     5.0 MB/sec    1.00      8.2±0.07ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1631.6±24.47µs    10.2 MB/sec    1.01  1647.8±27.58µs    10.1 MB/sec
formatter/numpy/globals.py                 1.00    181.9±0.29µs    16.2 MB/sec    1.01    183.7±4.09µs    16.1 MB/sec
formatter/pydantic/types.py                1.01      3.6±0.11ms     7.2 MB/sec    1.00      3.5±0.07ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.00     11.2±0.13ms     3.6 MB/sec    1.00     11.2±0.12ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.02ms     6.0 MB/sec    1.00      2.8±0.01ms     5.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    383.9±0.75µs     7.7 MB/sec    1.00    383.3±0.86µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.0±0.07ms     5.1 MB/sec    1.00      5.0±0.08ms     5.1 MB/sec
linter/default-rules/large/dataset.py      1.01      5.9±0.07ms     6.9 MB/sec    1.00      5.8±0.07ms     7.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1207.5±6.05µs    13.8 MB/sec    1.00  1212.6±14.22µs    13.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    134.9±0.54µs    21.9 MB/sec    1.00    134.7±1.16µs    21.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.6±0.02ms     9.9 MB/sec    1.01      2.6±0.06ms     9.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.11     10.9±0.19ms     3.7 MB/sec    1.00      9.8±0.11ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.09      2.1±0.05ms     8.1 MB/sec    1.00  1898.7±64.19µs     8.8 MB/sec
formatter/numpy/globals.py                 1.04    210.0±2.91µs    14.1 MB/sec    1.00    202.4±9.26µs    14.6 MB/sec
formatter/pydantic/types.py                1.09      4.6±0.04ms     5.5 MB/sec    1.00      4.2±0.05ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.4±0.17ms     3.0 MB/sec    1.00     13.4±0.13ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.05ms     4.7 MB/sec    1.00      3.6±0.03ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   371.5±10.29µs     7.9 MB/sec    1.00   371.3±38.60µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.07ms     4.2 MB/sec    1.01      6.1±0.11ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.3±0.10ms     5.5 MB/sec    1.00      7.3±0.07ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1465.0±21.20µs    11.4 MB/sec    1.00  1451.3±16.40µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    144.9±9.00µs    20.4 MB/sec    1.00    143.1±1.59µs    20.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.05ms     8.0 MB/sec    1.00      3.2±0.02ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
