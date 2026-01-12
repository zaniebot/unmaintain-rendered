```yaml
number: 6259
title: "Introduce an `Arguments` AST node for function calls and class definitions"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/class-base-comments
created_at: 2023-08-01T22:48:33Z
updated_at: 2023-08-03T13:26:57Z
url: https://github.com/astral-sh/ruff/pull/6259
synced_at: 2026-01-12T02:52:03Z
```

# Introduce an `Arguments` AST node for function calls and class definitions

---

_Pull request opened by @charliermarsh on 2023-08-01 22:48_

## Summary

This PR adds a new `Arguments` AST node, which we can use for function calls and class definitions.

The `Arguments` node spans from the left (open) to right (close) parentheses inclusive.

In the case of classes, the `Arguments` is an option, to differentiate between:

```python
# None
class C: ...

# Some, with empty vectors
class C(): ...
```

In this PR, we don't really leverage this change (except that a few rules get much simpler, since we don't need to lex to find the start and end ranges of the parentheses, e.g., `crates/ruff/src/rules/pyupgrade/rules/lru_cache_without_parameters.rs`, `crates/ruff/src/rules/pyupgrade/rules/unnecessary_class_parentheses.rs`).

In future PRs, this will be especially helpful for the formatter, since we can track comments enclosed on the node itself.

## Test Plan

`cargo test`


---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-01 22:48_

---

_Review requested from @zanieb by @charliermarsh on 2023-08-01 22:48_

---

_Review requested from @konstin by @charliermarsh on 2023-08-01 22:48_

---

_@charliermarsh reviewed on 2023-08-01 22:49_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/redundant_open_modes.rs`:127 on 2023-08-01 22:49_

I tended to favor destructuring to minimize code changes (i.e., we didn't have to change this method at all, only the `let` match here).

---

_@charliermarsh reviewed on 2023-08-01 22:49_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/lru_cache_without_parameters.rs`:83 on 2023-08-01 22:49_

This is an improvement (delete from left to right parens).

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/unnecessary_class_parentheses.rs`:56 on 2023-08-01 22:50_

This is way simpler (delete from left to right paren).

---

_@charliermarsh reviewed on 2023-08-01 22:50_

---

_Comment by @github-actions[bot] on 2023-08-01 22:59_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.4±0.11ms     4.9 MB/sec    1.00      8.3±0.06ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1634.6±14.00µs    10.2 MB/sec    1.00   1633.7±6.80µs    10.2 MB/sec
formatter/numpy/globals.py                 1.01    182.9±1.78µs    16.1 MB/sec    1.00    181.9±0.93µs    16.2 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.10ms     7.2 MB/sec    1.01      3.6±0.09ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.02     11.4±0.11ms     3.6 MB/sec    1.00     11.2±0.16ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.01ms     5.9 MB/sec    1.00      2.8±0.03ms     5.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    385.6±0.95µs     7.7 MB/sec    1.00    385.3±0.82µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.0±0.05ms     5.1 MB/sec    1.00      5.0±0.09ms     5.1 MB/sec
linter/default-rules/large/dataset.py      1.00      5.8±0.03ms     7.0 MB/sec    1.00      5.8±0.06ms     7.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1202.7±2.86µs    13.8 MB/sec    1.00   1197.9±7.69µs    13.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    133.6±0.27µs    22.1 MB/sec    1.00    132.5±0.30µs    22.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.6±0.01ms    10.0 MB/sec    1.00      2.5±0.03ms    10.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02     10.0±0.07ms     4.1 MB/sec    1.00      9.9±0.07ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.02  1934.6±19.59µs     8.6 MB/sec    1.00  1905.6±18.66µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    200.4±2.28µs    14.7 MB/sec    1.01    202.8±7.03µs    14.5 MB/sec
formatter/pydantic/types.py                1.02      4.3±0.04ms     5.9 MB/sec    1.00      4.2±0.03ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.4±0.09ms     3.0 MB/sec    1.00     13.5±0.10ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.6 MB/sec    1.00      3.6±0.02ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    364.8±5.04µs     8.1 MB/sec    1.00    363.4±5.01µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.05ms     4.2 MB/sec    1.00      6.1±0.04ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.05ms     5.6 MB/sec    1.01      7.4±0.05ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1459.4±11.95µs    11.4 MB/sec    1.02  1485.6±14.38µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    145.8±3.77µs    20.2 MB/sec    1.00    146.2±1.60µs    20.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.03ms     7.9 MB/sec    1.00      3.2±0.02ms     7.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-08-02 00:44_

(I am going to leverage these in the formatter in a separate change, to fix the trailing comment bug we have today.)

---

_@zanieb reviewed on 2023-08-02 03:36_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_builtins/rules/builtin_attribute_shadowing.rs`:85 on 2023-08-02 03:36_

Well this is a little less pretty.. would it make sense to add a helper to `ClassDef` to retrieve or iterate over bases? (in a future pull request)

---

_@charliermarsh reviewed on 2023-08-02 12:54_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_builtins/rules/builtin_attribute_shadowing.rs`:85 on 2023-08-02 12:54_

Yeah we can add a `bases` method. I can even do it here.

---

_@charliermarsh reviewed on 2023-08-02 12:59_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_builtins/rules/builtin_attribute_shadowing.rs`:85 on 2023-08-02 12:59_

Done.

---

_@konstin approved on 2023-08-02 13:32_

skimmed through it

---

_Merged by @charliermarsh on 2023-08-02 14:01_

---

_Closed by @charliermarsh on 2023-08-02 14:01_

---

_Branch deleted on 2023-08-02 14:01_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:175 on 2023-08-03 06:54_

Nit: We could return a `slice` here by using

```suggestion
    pub fn bases(&self) -> &[Expr] {
        match &self.arguments {
            Some(arguments) => &arguments.args,
            None => &[],
        }
    }
```

---

_@MichaReiser reviewed on 2023-08-03 06:54_

---

_@MichaReiser reviewed on 2023-08-03 06:55_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:3006 on 2023-08-03 06:55_

:eyes: 

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/nodes.rs`:175 on 2023-08-03 13:26_

Good call.

---

_@charliermarsh reviewed on 2023-08-03 13:26_

---
