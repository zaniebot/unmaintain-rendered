```yaml
number: 4648
title: Remove dedicated ScopeKind structs in favor of AST nodes
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/scope-structs
created_at: 2023-05-25T00:31:33Z
updated_at: 2023-05-25T19:53:55Z
url: https://github.com/astral-sh/ruff/pull/4648
synced_at: 2026-01-12T15:55:16Z
```

# Remove dedicated ScopeKind structs in favor of AST nodes

---

_@charliermarsh_

## Summary

This PR removes a variety of structs that we'd defined in `scope.rs` to enable precise typing of scope fields (e.g., `ScopeKind::ClassDef` should have a name, bases, etc.). Now that we have dedicated node types for each AST node (e.g., `ast::StmtClassDef`), we can just store those directly instead.

This change reduces the size of the `ScopeKind` enum from 104 bytes to 16 bytes. Unfortunately, that's a bit misleading, as I've moved the `globals` hash map off the enum and onto `Scope`, which is itself another 40 bytes -- so we're really going from 104 bytes to 56 bytes.


---

_@charliermarsh reviewed on 2023-05-25 00:35_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/scope.rs`:25 on 2023-05-25 00:35_

I actually have a branch that removes this, but it's a little less computationally efficient. We have to store these upfront so that we can detect `Expr::Name` expressions that reference a name _before_ it's bound via `global` statement -- that's a syntax error:

```py
def f():
  print(a)
  global a
```

The way we do this today is that we iterate over the function body to find all globals when we _first_ encounter the function (this isn't _too_ expensive, since we only have to iterate over statements, and don't need to recurse into other functions or classes), store them on the function scope, then, as we do the standard traversal, we check each `Expr::Name` against that global list.

An alternative, which wouldn't require us to store these at all, would be: (1) find all `global` statements, then (2) if we found _any_ global statements, do a full, dedicated traversal of the function body up to the location of the last `global` statement, to find any names that were used "too early".

That is: rather than finding the globals upfront, and then checking against names as we iterate over the rest of the AST, we'd find the globals upfront, then find all names upfront too if needed. In theory, this should be slower, though in practice, it may not matter? Since the vast majority of scopes don't have globals anyway.


---

_Comment by @github-actions[bot] on 2023-05-25 00:42_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.06     19.1±0.99ms     2.1 MB/sec    1.00     18.0±0.84ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.16      4.5±0.20ms     3.7 MB/sec    1.00      3.9±0.27ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.05   557.3±20.27µs     5.3 MB/sec    1.00   529.1±31.36µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.09      8.0±0.44ms     3.2 MB/sec    1.00      7.4±0.40ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.16      9.2±0.32ms     4.4 MB/sec    1.00      7.9±0.34ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.19  1949.5±48.21µs     8.5 MB/sec    1.00  1634.8±129.44µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.08   224.2±10.48µs    13.2 MB/sec    1.00   208.4±23.28µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.24      4.1±0.18ms     6.2 MB/sec    1.00      3.3±0.20ms     7.7 MB/sec
parser/large/dataset.py                    1.00      6.8±0.29ms     6.0 MB/sec    1.05      7.1±0.13ms     5.7 MB/sec
parser/numpy/ctypeslib.py                  1.00  1377.6±76.64µs    12.1 MB/sec    1.02  1404.3±30.76µs    11.9 MB/sec
parser/numpy/globals.py                    1.02    135.4±5.54µs    21.8 MB/sec    1.00    133.0±8.37µs    22.2 MB/sec
parser/pydantic/types.py                   1.00      3.0±0.10ms     8.6 MB/sec    1.01      3.0±0.12ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.9±0.20ms     2.4 MB/sec    1.02     17.3±0.27ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.06ms     3.9 MB/sec    1.03      4.4±0.10ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    502.6±9.35µs     5.9 MB/sec    1.00    504.8±7.11µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.11ms     3.6 MB/sec    1.00      7.1±0.12ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.4±0.09ms     4.9 MB/sec    1.00      8.3±0.07ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1789.2±26.42µs     9.3 MB/sec    1.00  1787.3±22.91µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.4±3.30µs    14.5 MB/sec    1.01    206.3±5.05µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.8 MB/sec    1.01      3.8±0.05ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.5±0.05ms     6.3 MB/sec    1.01      6.5±0.06ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1242.0±16.66µs    13.4 MB/sec    1.00  1244.4±15.76µs    13.4 MB/sec
parser/numpy/globals.py                    1.00    125.7±1.96µs    23.5 MB/sec    1.00    126.0±2.26µs    23.4 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.1 MB/sec    1.00      2.8±0.03ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/scope.rs`:25 on 2023-05-25 06:15_

What's the motivation for removing it? To reduce the size of `Scope`? One thing that we could do is to store the hash map out of bounds. Meaning, we only store a `Option<GlobalsId>` here (4 bytes vs 48 bytes) and store the globals in a `Vec` on scopes (indexed by `GlobalsId`). This adds one additional indirection (or we can store an `Option<Box<HashMap>>`. 

 

---

_@MichaReiser reviewed on 2023-05-25 06:15_

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:1687 on 2023-05-25 06:16_

Nit: `scope.kind.is_any_function()` ? 

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:1901 on 2023-05-25 06:19_

I think it could make sense for us to introduce a `AnyFunctionDef` union in `rust_python_ast` that has accessor methods for every field (that both `FunctionDef` and `AsyncFunctionDef` have. It can implement `From`, the `AstNode` trait, and `as/into_function_def()` and `as/into_async_function_def()` methods. 

We found this a very powerful pattern at Rome and I think that would simplify the usage here. 

https://github.com/rome/tools/blob/38104b339da8ff688f469799e1fc3f4a46f3d2ec/crates/rome_js_syntax/src/generated/nodes.rs#L13369-L13404
https://github.com/rome/tools/blob/38104b339da8ff688f469799e1fc3f4a46f3d2ec/crates/rome_js_syntax/src/union_ext.rs#L132-L238

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:1935 on 2023-05-25 06:21_

Part 1

```suggestion
            Stmt::ClassDef(class_def @ ast::StmtClassDef {
```


---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:1963 on 2023-05-25 06:22_

Part 2
```suggestion
                self.semantic_model.push_scope(ScopeKind::Class(class_def));
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_simplify/rules/ast_unary_op.rs`:102 on 2023-05-25 06:23_

Do we need to distinguish between `Function` and `AsyncFunction`. If not, having a single `AnyFunction` kind might be easier to use (it will increase the enum size by 8 bytes because it now requires two discriminates).

---

_@MichaReiser approved on 2023-05-25 06:25_

I like it. I think we could improve the economics by introducing an `AnyFunctionDef` enum that abstracts away `Function` and `AsyncFunction` (still allows to retrieve it when necessary, but it doesn't seem to matter for 99% of the uses)

---

_@charliermarsh reviewed on 2023-05-25 15:37_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/scope.rs`:25 on 2023-05-25 15:37_

Yeah, to reduce the size of `Scope`. That's an interesting idea, I'll try it.

---

_@charliermarsh reviewed on 2023-05-25 15:38_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:1901 on 2023-05-25 15:38_

What if we just used a single `FunctionDef` node with an `async_` property?

---

_@MichaReiser reviewed on 2023-05-25 15:39_

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:1901 on 2023-05-25 15:39_

That's not CPython compatible ;)

---

_@charliermarsh reviewed on 2023-05-25 19:23_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:1935 on 2023-05-25 19:23_

Ugh, I know, I just hated that it wasn't symmetric with the function case :sob:

---

_@charliermarsh reviewed on 2023-05-25 19:25_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:1901 on 2023-05-25 19:25_

Pshhhh

---

_@charliermarsh reviewed on 2023-05-25 19:25_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_simplify/rules/ast_unary_op.rs`:102 on 2023-05-25 19:25_

Leaving this for now, but what do you mean by "two discriminates" here?

---

_@charliermarsh reviewed on 2023-05-25 19:26_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/scope.rs`:25 on 2023-05-25 19:26_

(Will explore in a separate PR.)

---

_Merged by @charliermarsh on 2023-05-25 19:31_

---

_Closed by @charliermarsh on 2023-05-25 19:31_

---

_Branch deleted on 2023-05-25 19:31_

---
