```yaml
number: 4593
title: "Track `TYPE_CHECKING` blocks in `Importer`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/type-checking-blocks
created_at: 2023-05-23T02:12:43Z
updated_at: 2023-05-30T16:48:31Z
url: https://github.com/astral-sh/ruff/pull/4593
synced_at: 2026-01-12T03:50:03Z
```

# Track `TYPE_CHECKING` blocks in `Importer`

---

_Pull request opened by @charliermarsh on 2023-05-23 02:12_

## Summary

We aren't making use of these yet, but this enables us to track `if TYPE_CHECKING:` blocks in the `Importer`, which is necessary to facilitate moving imports from runtime- to typing-only contexts.

For example:

```py
import os
import typing

if TYPE_CHECKING:
    import tensorflow as tf
```

In this case, we'd track the two top-level imports in `runtime_imports`, plus the if-statement in `type_checking_blocks`.


---

_Comment by @github-actions[bot] on 2023-05-23 02:29_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.6±0.11ms     2.8 MB/sec    1.00     14.6±0.10ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.02ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    428.3±0.59µs     6.9 MB/sec    1.00    426.2±0.61µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.0±0.07ms     4.2 MB/sec    1.00      6.0±0.04ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.01      7.0±0.04ms     5.9 MB/sec    1.00      6.9±0.04ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1520.8±2.77µs    10.9 MB/sec    1.00  1511.9±37.66µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    172.9±1.41µs    17.1 MB/sec    1.00    172.1±1.19µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.01ms     8.1 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.2±0.01ms     7.9 MB/sec    1.00      5.2±0.01ms     7.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1025.6±1.92µs    16.2 MB/sec    1.00   1023.8±1.44µs    16.3 MB/sec
parser/numpy/globals.py                    1.01    106.5±2.88µs    27.7 MB/sec    1.00    105.4±0.25µs    28.0 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.4 MB/sec    1.00      2.2±0.00ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.6±0.55ms     2.3 MB/sec    1.01     17.7±0.90ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.6±0.26ms     3.6 MB/sec    1.00      4.6±0.22ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.03   558.9±39.02µs     5.3 MB/sec    1.00   545.1±30.12µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.5±0.34ms     3.4 MB/sec    1.02      7.6±0.42ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.8±0.31ms     4.6 MB/sec    1.00      8.8±0.33ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1909.8±83.65µs     8.7 MB/sec    1.00  1885.4±112.56µs     8.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   226.4±14.16µs    13.0 MB/sec    1.05   237.9±27.75µs    12.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.20ms     6.4 MB/sec    1.01      4.0±0.39ms     6.3 MB/sec
parser/large/dataset.py                    1.00      7.0±0.27ms     5.8 MB/sec    1.02      7.2±0.35ms     5.7 MB/sec
parser/numpy/ctypeslib.py                  1.00  1371.6±60.84µs    12.1 MB/sec    1.00  1364.8±75.97µs    12.2 MB/sec
parser/numpy/globals.py                    1.00    135.7±7.09µs    21.7 MB/sec    1.05    142.2±8.79µs    20.8 MB/sec
parser/pydantic/types.py                   1.00      3.1±0.17ms     8.3 MB/sec    1.01      3.1±0.14ms     8.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @charliermarsh on 2023-05-30 01:09_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-30 01:09_

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:2087 on 2023-05-30 06:27_

Should we pass the entire if statement here? it would prevent from calling this function with e.g. a `test` condition from a while or any other expression (where the result would not make sense)


---

_Review comment by @MichaReiser on `crates/ruff/src/importer/mod.rs`:29 on 2023-05-30 06:29_

Is this supposed to be TYPE_CHECKING?

```suggestion
    /// The list of visited, top-level `if TYPE_CHECKING:` blocks in the Python AST.
```


---

_Review comment by @MichaReiser on `crates/ruff/src/importer/mod.rs`:54 on 2023-05-30 06:30_

Nit: Change to `StmtIf` because that's the only block that we support?

```suggestion
    pub(crate) fn visit_type_checking_block(&mut self, type_checking_block: &'a StmtIf) {
```

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:2087 on 2023-05-30 06:30_

```suggestion
                if analyze::typing::is_type_checking_block(test, &self.semantic_model {
```

Nit: I would change the order of the arguments so that it reads nicely: The function tests if `test` *is a type checking block*. The semantic model plays a less important role. 

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:2087 on 2023-05-30 06:31_

Wishlist: It would be nice if we could cache the `resolve_call_path` calls for as long as it is the same scope

---

_@MichaReiser approved on 2023-05-30 06:32_

---

_@charliermarsh reviewed on 2023-05-30 13:20_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:2087 on 2023-05-30 13:20_

I agree...

---

_Review comment by @charliermarsh on `crates/ruff/src/importer/mod.rs`:29 on 2023-05-30 16:05_

Copilot :joy:

---

_@charliermarsh reviewed on 2023-05-30 16:06_

---

_Merged by @charliermarsh on 2023-05-30 16:18_

---

_Closed by @charliermarsh on 2023-05-30 16:18_

---

_Branch deleted on 2023-05-30 16:18_

---
