```yaml
number: 6314
title: Add support for simple generic type variables to UP040
type: pull_request
state: merged
author: zanieb
labels:
  - fixes
assignees: []
merged: true
base: main
head: rule/type-alias-generic
created_at: 2023-08-03T18:35:37Z
updated_at: 2023-08-07T21:23:20Z
url: https://github.com/astral-sh/ruff/pull/6314
synced_at: 2026-01-12T15:55:21Z
```

# Add support for simple generic type variables to UP040

---

_@zanieb_

Extends #6289 to support moving type variable usage in type aliases to use PEP-695.

Does not remove the possibly unused type variable declaration. ~Presumably this is handled by other rules, but is not working for me.~ We cannot remove module level declarations safely.

Does not handle type variables with bounds or variance declarations yet.

Part of #4617 

---

_Comment by @github-actions[bot] on 2023-08-03 18:49_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.3±0.13ms     4.9 MB/sec    1.00      8.2±0.17ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.01  1644.6±44.20µs    10.1 MB/sec    1.00  1625.4±30.34µs    10.2 MB/sec
formatter/numpy/globals.py                 1.02    186.0±6.90µs    15.9 MB/sec    1.00    181.8±3.22µs    16.2 MB/sec
formatter/pydantic/types.py                1.01      3.5±0.06ms     7.3 MB/sec    1.00      3.4±0.07ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.00     10.1±0.04ms     4.0 MB/sec    1.00     10.1±0.02ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.02ms     6.2 MB/sec    1.01      2.7±0.01ms     6.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    377.2±1.39µs     7.8 MB/sec    1.01    380.7±1.97µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.6±0.04ms     5.5 MB/sec    1.00      4.6±0.03ms     5.5 MB/sec
linter/default-rules/large/dataset.py      1.00      5.2±0.01ms     7.8 MB/sec    1.00      5.2±0.01ms     7.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1123.8±2.80µs    14.8 MB/sec    1.01   1132.3±4.46µs    14.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    127.7±1.28µs    23.1 MB/sec    1.01    128.9±2.59µs    22.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.3±0.00ms    10.9 MB/sec    1.01      2.3±0.01ms    10.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.05     10.3±0.07ms     3.9 MB/sec    1.00      9.8±0.08ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.05  1952.7±26.25µs     8.5 MB/sec    1.00  1863.1±18.09µs     8.9 MB/sec
formatter/numpy/globals.py                 1.03    197.0±3.39µs    15.0 MB/sec    1.00    191.4±3.45µs    15.4 MB/sec
formatter/pydantic/types.py                1.05      4.4±0.03ms     5.8 MB/sec    1.00      4.2±0.10ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     12.6±0.14ms     3.2 MB/sec    1.00     12.6±0.19ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.03ms     4.7 MB/sec    1.00      3.5±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    366.8±5.68µs     8.0 MB/sec    1.00    362.5±7.92µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.04ms     4.4 MB/sec    1.01      5.9±0.04ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.09ms     6.1 MB/sec    1.02      6.8±0.04ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1384.0±10.99µs    12.0 MB/sec    1.01  1391.7±15.61µs    12.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    139.9±1.84µs    21.1 MB/sec    1.01    141.2±2.18µs    20.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.07ms     8.5 MB/sec    1.01      3.0±0.02ms     8.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-08-03 19:56_

> Does not remove the possibly unused type variable declaration. Presumably this is handled by other rules, but is not working for me.

We don't remove unused variables in the module scope, since they could be imported from other modules (i.e., they are part of the module's public API). (We only remove unused variables within function scopes.)

---

_Comment by @zanieb on 2023-08-03 20:10_

> We don't remove unused variables in the module scope, since they could be imported from other modules (i.e., they are part of the module's public API). (We only remove unused variables within function scopes.)

Ah that makes sense. We're pretty unlikely to ever remove these then. I don't think we should either, I've definitely imported them :D

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyupgrade/rules/use_pep695_type_alias.rs`:134 on 2023-08-03 20:20_

Nit: You can use `and_then` to avoid `Option<Option<...>`
```suggestion
                        .and_then(|binding_id| {
```

---

_@MichaReiser reviewed on 2023-08-03 20:22_

---

_Review comment by @zanieb on `crates/ruff/src/rules/pyupgrade/rules/use_pep695_type_alias.rs`:134 on 2023-08-03 20:25_

Ah thanks that was awkward

---

_@zanieb reviewed on 2023-08-03 20:25_

---

_@charliermarsh reviewed on 2023-08-04 01:20_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/use_pep695_type_alias.rs`:2 on 2023-08-04 01:20_

We often do `use ruff_python_ast::{self as ast, ...}` for convenience / by convention. Then you can do `ast::TypeParams` instead of `ruff_python_ast::TypeParams` below.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/use_pep695_type_alias.rs`:121 on 2023-08-04 01:20_

Nit: `#[derive(Debug)]` for any structs that can.

---

_@charliermarsh reviewed on 2023-08-04 01:20_

---

_@charliermarsh reviewed on 2023-08-04 01:21_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/use_pep695_type_alias.rs`:125 on 2023-08-04 01:21_

Nit: we typically call this argument `model`.

---

_@charliermarsh reviewed on 2023-08-04 01:22_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/use_pep695_type_alias.rs`:133 on 2023-08-04 01:22_

You may want `self.semantic.lookup_symbol` here? Unless you're intentionally not looking beyond the current scope. Or perhaps you _do_ want class variable assignments to resolve here?

---

_@charliermarsh approved on 2023-08-04 01:22_

---

_Label `autofix` added by @charliermarsh on 2023-08-04 01:22_

---

_@zanieb reviewed on 2023-08-04 13:56_

---

_Review comment by @zanieb on `crates/ruff/src/rules/pyupgrade/rules/use_pep695_type_alias.rs`:125 on 2023-08-04 13:56_

A search for `semantic.` gives 303 results while `model.` gives 28 results / `semantic: &Semantic` gives 99 results while `model: &Semantic` gives 13. You've told me this before but it doesn't seem to be the case. Happy to consolidate on a standard though.

---

_@zanieb reviewed on 2023-08-04 13:57_

---

_Review comment by @zanieb on `crates/ruff/src/rules/pyupgrade/rules/use_pep695_type_alias.rs`:133 on 2023-08-04 13:57_

Oh gosh... I'll need to think about this. I took this from somewhere else.

---

_Review comment by @zanieb on `crates/ruff/src/rules/pyupgrade/rules/use_pep695_type_alias.rs`:121 on 2023-08-07 18:59_

Cannot be derived

```
error[E0277]: `SemanticModel<'a>` doesn't implement `std::fmt::Debug`
   --> crates/ruff/src/rules/pyupgrade/rules/use_pep695_type_alias.rs:125:5
    |
122 | #[derive(Debug)]
    |          ----- in this derive macro expansion
...
125 |     semantic: &'a SemanticModel<'a>,
    |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ `SemanticModel<'a>` cannot be formatted using `{:?}` because it doesn't implement `std::fmt::Debug`
    |
   ```

---

_@zanieb reviewed on 2023-08-07 18:59_

---

_@MichaReiser reviewed on 2023-08-07 19:03_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyupgrade/rules/use_pep695_type_alias.rs`:125 on 2023-08-07 19:03_

I prefer semantic or semantic_model. Model is too generic in my view (you need an IDE to know what the variable is)

---

_@charliermarsh reviewed on 2023-08-07 19:12_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/use_pep695_type_alias.rs`:125 on 2023-08-07 19:12_

I must be mixing up the convention, feel free to move forward with `semantic: &Semantic`, my mistake.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/use_pep695_type_alias.rs`:133 on 2023-08-07 19:57_

Does this need to match `AnnAssign` too? No, right?

---

_@charliermarsh reviewed on 2023-08-07 19:58_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/use_pep695_type_alias.rs`:125 on 2023-08-07 20:01_

https://github.com/astral-sh/ruff/pull/6406

---

_@charliermarsh reviewed on 2023-08-07 20:01_

---

_@zanieb reviewed on 2023-08-07 20:42_

---

_Review comment by @zanieb on `crates/ruff/src/rules/pyupgrade/rules/use_pep695_type_alias.rs`:133 on 2023-08-07 20:42_

I don't think so. At least, Pyright complains about

```python
from typing import TypeVar

T: TypeVar = TypeVar("T")
```

---

_Merged by @zanieb on 2023-08-07 21:22_

---

_Closed by @zanieb on 2023-08-07 21:22_

---

_Branch deleted on 2023-08-07 21:22_

---
