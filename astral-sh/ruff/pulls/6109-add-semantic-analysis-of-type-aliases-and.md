```yaml
number: 6109
title: Add semantic analysis of type aliases and parameters
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/695-semantic
created_at: 2023-07-26T22:22:11Z
updated_at: 2023-07-28T22:06:38Z
url: https://github.com/astral-sh/ruff/pull/6109
synced_at: 2026-01-12T15:55:20Z
```

# Add semantic analysis of type aliases and parameters

---

_@zanieb_

Requires https://github.com/astral-sh/RustPython-Parser/pull/42
Related https://github.com/PyCQA/pyflakes/pull/778
[PEP-695](https://peps.python.org/pep-0695)
Part of #5062 

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Adds a scope for type parameters, a type parameter binding kind, and checker visitation of type parameters in type alias statements, function definitions, and class definitions.

A few changes were necessary to ensure correctness following the insertion of a new scope between function and class scopes and their parent.

## Test Plan

<!-- How was it tested? -->
Undefined name snapshots.

Unused type parameter rule will be added as follow-up.

---

_Comment by @zanieb on 2023-07-26 22:32_

If interested, working with examples at https://gist.github.com/zanieb/fe1d6c32db2e9a6cc1bcb3be6feaaf05

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:1749 on 2023-07-26 22:46_

Nit: you might be able to do this in the match? Like `ast::TypeParam::TypeVar(ast::TypeParamTypeVar { Some(bound), .. }) => {`? But I didn't try it.

---

_@charliermarsh reviewed on 2023-07-26 22:46_

---

_Comment by @charliermarsh on 2023-07-26 22:46_

This is looking good to me!

---

_@zanieb reviewed on 2023-07-26 22:54_

---

_Review comment by @zanieb on `crates/ruff/src/checkers/ast/mod.rs`:1749 on 2023-07-26 22:54_

I think it's boxed so it doesn't work.

---

_@zanieb reviewed on 2023-07-26 22:57_

---

_Review comment by @zanieb on `crates/ruff/src/checkers/ast/mod.rs`:1749 on 2023-07-26 22:57_

Not related to the box but it does fail
```❯ cc
    Checking ruff v0.0.280 (/Users/mz/eng/src/astral-sh/ruff/crates/ruff)
error: expected `,`
    --> crates/ruff/src/checkers/ast/mod.rs:1748:73
     |
1748 |                     ast::TypeParam::TypeVar(ast::TypeParamTypeVar { Some(bound), .. }) => {
     |                                             ---------------------       ^
     |                                             |
     |                                             while parsing the fields for this pattern

error[E0425]: cannot find value `bound` in this scope```

---

_@zanieb reviewed on 2023-07-26 22:58_

---

_Review comment by @zanieb on `crates/ruff/src/checkers/ast/mod.rs`:1749 on 2023-07-26 22:58_

Ah duh

```rust
ast::TypeParam::TypeVar(ast::TypeParamTypeVar { bound: Some(bound), .. }) => {
```

---

_Comment by @github-actions[bot] on 2023-07-26 23:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.19     12.9±0.38ms     3.1 MB/sec     1.00     10.9±0.47ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.19      2.5±0.12ms     6.7 MB/sec     1.00      2.1±0.11ms     8.0 MB/sec
formatter/numpy/globals.py                 1.05   262.1±11.92µs    11.3 MB/sec     1.00   249.6±23.05µs    11.8 MB/sec
formatter/pydantic/types.py                1.13      5.3±0.29ms     4.8 MB/sec     1.00      4.7±0.26ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     14.2±0.68ms     2.9 MB/sec     1.00     14.3±0.53ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.09ms     4.8 MB/sec     1.06      3.7±0.11ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   493.1±19.60µs     6.0 MB/sec     1.02   502.4±17.31µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.22ms     4.1 MB/sec     1.03      6.5±0.29ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.26ms     5.4 MB/sec     1.07      8.1±0.28ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.07  1731.7±213.19µs     9.6 MB/sec    1.00  1612.3±81.91µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    177.2±6.23µs    16.7 MB/sec     1.10   194.9±17.64µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.14ms     7.6 MB/sec     1.04      3.5±0.14ms     7.3 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00     13.7±0.71ms     3.0 MB/sec     1.01     13.9±0.65ms     2.9 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.7±0.17ms     6.3 MB/sec     1.00      2.6±0.18ms     6.3 MB/sec
formatter/numpy/globals.py                 1.02   294.9±33.43µs    10.0 MB/sec     1.00   289.6±21.05µs    10.2 MB/sec
formatter/pydantic/types.py                1.01      5.8±0.42ms     4.4 MB/sec     1.00      5.7±0.27ms     4.5 MB/sec
linter/all-rules/large/dataset.py          1.00     17.9±0.71ms     2.3 MB/sec     1.00     17.9±0.73ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.6±0.31ms     3.6 MB/sec     1.02      4.7±0.32ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   545.8±38.41µs     5.4 MB/sec     1.01   549.6±34.32µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.9±0.36ms     3.2 MB/sec     1.01      8.0±0.48ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      9.7±0.41ms     4.2 MB/sec     1.02      9.9±0.54ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1957.4±102.10µs     8.5 MB/sec    1.01  1983.9±145.44µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.01   215.0±18.54µs    13.7 MB/sec     1.00   212.6±13.41µs    13.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.32ms     5.9 MB/sec     1.03      4.5±0.41ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @zanieb on 2023-07-27 21:46_

---

_Comment by @zanieb on 2023-07-27 21:55_

Huh my latest commit is pushed but missing. Silly GitHub.

---

_@charliermarsh reviewed on 2023-07-28 01:57_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/pyflakes/F821_17.py`:17 on 2023-07-28 01:57_

Were you able to test these against the updated version of Pyflakes?

---

_@charliermarsh reviewed on 2023-07-28 01:59_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyflakes/mod.rs`:1276 on 2023-07-28 01:59_

Nit: looks like some trailing whitespace here.

---

_@charliermarsh reviewed on 2023-07-28 02:02_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:874 on 2023-07-28 02:02_

Hmm, maybe `non_type_scope_parent`, so that it describes the data returned rather than the action taken? But not a strong opinion.

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:882 on 2023-07-28 02:02_

I think this can be written as a `while-let` loop:

```rust
/// Returns the first parent of the given scope that is not a [`ScopeKind::Type`] scope, if any.
pub fn scope_parent_skip_types(&self, scope: &Scope) -> Option<&Scope<'a>> {
    let mut current_scope = scope;
    while let Some(parent) = self.scope_parent(current_scope) {
        if parent.kind.is_type() {
            current_scope = parent;
        } else {
            return Some(parent);
        }
    }
    None
}
```

---

_@charliermarsh reviewed on 2023-07-28 02:02_

---

_@charliermarsh reviewed on 2023-07-28 02:04_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/pyflakes/F821_17.py`:102 on 2023-07-28 02:04_

Nit: I think this file is missing a trailing newline.

---

_@charliermarsh approved on 2023-07-28 02:05_

This looks great! I love the thoroughness of the test suite. I admittedly didn't review the actual test results for semantic correctness but am happy to take a closer look on any of the finer points if useful.

---

_@zanieb reviewed on 2023-07-28 03:32_

---

_Review comment by @zanieb on `crates/ruff_python_semantic/src/model.rs`:882 on 2023-07-28 03:32_

Ah cool didn't know you could do that, will try.

---

_@zanieb reviewed on 2023-07-28 03:33_

---

_Review comment by @zanieb on `crates/ruff_python_semantic/src/model.rs`:874 on 2023-07-28 03:33_

I'm not attached to my name but I don't like that one either haha maybe `scope_non_type_parent`? eek. I also wanted to try `scope_first_parent_not(kind: ScopeKind)` or something but making it generic felt weird too.

---

_@zanieb reviewed on 2023-07-28 03:34_

---

_Review comment by @zanieb on `crates/ruff/resources/test/fixtures/pyflakes/F821_17.py`:17 on 2023-07-28 03:34_

I haven't! I'll do that and confirm they match. I tested for correctness with Pyright and Python runtime errors.

---

_@zanieb reviewed on 2023-07-28 03:49_

---

_Review comment by @zanieb on `crates/ruff/resources/test/fixtures/pyflakes/F821_17.py`:31 on 2023-07-28 03:49_

Self: Need to move `Forward` definition to end of file or change names so they do not collide across test cases.

---

_@charliermarsh reviewed on 2023-07-28 04:43_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/pyflakes/F821_17.py`:31 on 2023-07-28 04:43_

Another benefit of isolating fixtures >.<

---

_@charliermarsh reviewed on 2023-07-28 04:45_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:874 on 2023-07-28 04:45_

Maybe what you have here is just clearest... Or `scope_parent_skip_type_scopes`???

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:874 on 2023-07-28 06:30_

Maybe `first_non_type_parent_scope` or `non_type_parent_scope`?

Or we name it `first_value_scope`. The way I think about it is that variables can either bind a *value* or a *type*. 

Unrelated: Is this a python thing that types have their own scope? In JS, they all end up in the same scope, but each binding is either a *value* or a *type*. 


---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:869 on 2023-07-28 06:31_

Nit: It should be `parent_scope` to be consistent with `global_scope`. I also find it reads more naturally. It's the parent scope. Or you need to use an apostrophe: `scope's_parent`.

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:882 on 2023-07-28 06:35_

Or you use `std::iter::successors`

```rust
std::iter::successors(self.parent_scope(current_scope), |scope| self.parent_scope(scope)).find(|scope| !scope.kind.is_type()) 
```

Being able to iterate over all `ancestor_scopes` may be generally useful and would improve readability here:


```rust
pub fn ancestor_scopes(&self, scope: &Scope) -> impl Iterator<Item = &Scope> {
	std::iter::successors(self.scope_parent(scope), |next| self.scope_parent(next)) 
} 

pub fn first_value_scope(&self, scope: &Scope) -> Option<&Scope> {
	self.ancestor_scopes(scope).find(|scope| !scope.kind.is_type())
}
```

---

_@MichaReiser reviewed on 2023-07-28 06:37_

---

_@charliermarsh reviewed on 2023-07-28 12:34_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:874 on 2023-07-28 12:34_

Yeah, surprisingly they added an entirely new lexical scope to support type parameters specifically in PEP 695: https://peps.python.org/pep-0695/#type-parameter-scopes. It's required due to some of the nuances of when these things are evaluated.

---

_@charliermarsh reviewed on 2023-07-28 12:35_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:874 on 2023-07-28 12:35_

(Perhaps we should consider a more specific name for that scope kind though.)

---

_@charliermarsh reviewed on 2023-07-28 12:37_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:882 on 2023-07-28 12:37_

FWIW we have `ancestors` on the `Scopes` struct, but it needs a `ScopeId` not a `Scope`, and we don't actually store the ID on `Scope`. So we'd need to refactor to pass the ID here rather than the `Scope` itself.

---

_@zanieb reviewed on 2023-07-28 17:46_

---

_Review comment by @zanieb on `crates/ruff_python_semantic/src/model.rs`:874 on 2023-07-28 17:46_

I kind of like `TypeParam` scope more than `Type` but I didn't want to deviate in this first pass

---

_Review comment by @zanieb on `crates/ruff_python_semantic/src/model.rs`:869 on 2023-07-28 17:49_

Hmm I wrote it that way first but I was trying to be consistent with `expr_parent` / `expr_grandparent` / `stmt_parent`. The `global_scope` is a special kind of scope, not a scope relative to the current one.

---

_@zanieb reviewed on 2023-07-28 17:49_

---

_@zanieb reviewed on 2023-07-28 17:52_

---

_Review comment by @zanieb on `crates/ruff_python_semantic/src/model.rs`:882 on 2023-07-28 17:52_

Unsure how to proceed here. I'm not really sure how we want to deal with overlap on the `Scopes` struct.

---

_@charliermarsh reviewed on 2023-07-28 18:22_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:882 on 2023-07-28 18:22_

I think using a loop is fine for now, we can always consider this refactor later as it's somewhat orthogonal.

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:869 on 2023-07-28 18:24_

(The naming conventions on the model API are not very good and have evolved somewhat haphazardly.)

---

_@charliermarsh reviewed on 2023-07-28 18:24_

---

_@zanieb reviewed on 2023-07-28 20:15_

---

_Review comment by @zanieb on `crates/ruff_python_semantic/src/model.rs`:869 on 2023-07-28 20:15_

I'll just change them for now. Not worth going back and forth I think. We can address it more later if needed.

---

_Review comment by @zanieb on `crates/ruff/resources/test/fixtures/pyflakes/F821_17.py`:17 on 2023-07-28 22:06_

The results are identical except that pyflakes emits two incorrect warnings

```
10:21: undefined name 'Ts'
11:21: undefined name 'P'
```

They appear to handle binding of type var tuples and param specs incorrectly.

---

_@zanieb reviewed on 2023-07-28 22:06_

---

_Merged by @zanieb on 2023-07-28 22:06_

---

_Closed by @zanieb on 2023-07-28 22:06_

---

_Branch deleted on 2023-07-28 22:06_

---
