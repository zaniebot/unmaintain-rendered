```yaml
number: 4339
title: "Delay computation of `Definition` visibility"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/definitions
created_at: 2023-05-10T03:27:56Z
updated_at: 2023-05-11T17:39:43Z
url: https://github.com/astral-sh/ruff/pull/4339
synced_at: 2026-01-12T15:55:15Z
```

# Delay computation of `Definition` visibility

---

_@charliermarsh_

## Summary

The AST checker has a concept of "definitions", defined as parts of the AST that can be documented with a docstring. In practice, this overlaps with scopes, since everything that can be documented also creates a new scope -- but in my opinion, it's helpful to treat them as separate concepts given that (1) not all constructs that create scopes can be documented (like comprehensions and lambdas), and (2) there are in theory other things that can be documented (like class attributes) that don't create scopes.

Anyway, in order to determine whether a definition is missing a docstring, we need to assess the definition's visibility: is it a public or private function/class/module? Visibility is a concept that relies on parent-child relationships (nested classes within private classes are private; nested classes within public classes _can_ be public). Historically, we tracked these visibility rules as we iterated over the AST.

However, we now need to adjust our visibility rules to respect `__all__` -- if a module defines `__all__`, and a definition is excluded from `__all__`, it should be considered private. Since `__all__` can be modified throughout the program, it's no longer possible to track visibility as we go.

This PR refactors the "definitions" concept such that we now track definitions as we go, and then compute visibility after we've traversed the entire program. In the process, I also removed a few abstractions and refactored the `Definition` struct hierarchy to better reflect the layout of the data (i.e., we have separate enums for modules vs. members).

Note that we're not yet respecting `__all__` -- that will come in a separate PR. But this PR makes it "trivial" to add that behavior.

## Test Plan

Can rely entirely on our automated tests here. There should be no behavioral changes.


---

_Comment by @charliermarsh on 2023-05-10 03:49_

(Draft as I need to break this into a few PRs.)

---

_Comment by @github-actions[bot] on 2023-05-10 04:03_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     18.9±0.47ms     2.2 MB/sec    1.00     18.7±0.48ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.12ms     3.8 MB/sec    1.01      4.5±0.14ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   547.7±17.30µs     5.4 MB/sec    1.01   555.2±16.18µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.7±0.25ms     3.3 MB/sec    1.01      7.8±0.22ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      9.0±0.21ms     4.5 MB/sec    1.00      9.0±0.22ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1858.4±44.38µs     9.0 MB/sec    1.01  1881.7±37.95µs     8.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    220.8±6.29µs    13.4 MB/sec    1.01    222.5±8.79µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.11ms     6.4 MB/sec    1.02      4.0±0.13ms     6.3 MB/sec
parser/large/dataset.py                    1.05      7.6±0.19ms     5.3 MB/sec    1.00      7.3±0.16ms     5.6 MB/sec
parser/numpy/ctypeslib.py                  1.05  1482.0±32.74µs    11.2 MB/sec    1.00  1405.4±45.03µs    11.8 MB/sec
parser/numpy/globals.py                    1.04    145.7±4.68µs    20.3 MB/sec    1.00    139.9±6.28µs    21.1 MB/sec
parser/pydantic/types.py                   1.04      3.2±0.09ms     7.9 MB/sec    1.00      3.1±0.06ms     8.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.5±0.23ms     2.5 MB/sec    1.00     16.5±0.23ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     4.0 MB/sec    1.00      4.2±0.08ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   493.1±10.49µs     6.0 MB/sec    1.00   492.3±12.72µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.13ms     3.7 MB/sec    1.00      6.9±0.12ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.09ms     4.9 MB/sec    1.00      8.2±0.13ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1749.3±42.39µs     9.5 MB/sec    1.00  1741.2±29.32µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    201.4±8.41µs    14.7 MB/sec    1.00    199.3±9.14µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.11ms     6.9 MB/sec    1.00      3.7±0.05ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.7±0.07ms     6.1 MB/sec    1.01      6.8±0.08ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1262.3±18.36µs    13.2 MB/sec    1.01  1275.0±18.66µs    13.1 MB/sec
parser/numpy/globals.py                    1.00    128.3±1.92µs    23.0 MB/sec    1.01    129.4±2.25µs    22.8 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.05ms     9.0 MB/sec    1.01      2.9±0.03ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-10 17:23_

---

_Review requested from @konstin by @charliermarsh on 2023-05-10 17:23_

---

_Marked ready for review by @charliermarsh on 2023-05-10 17:23_

---

_@charliermarsh reviewed on 2023-05-10 17:24_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/deferred.rs`:13 on 2023-05-10 17:24_

`VisibleScope` is gone, no longer needs to exist.

---

_@charliermarsh reviewed on 2023-05-10 17:24_

---

_Review comment by @charliermarsh on `crates/ruff/src/docstrings/mod.rs`:16 on 2023-05-10 17:24_

This was moved out of `definition.rs`, unchanged.

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/definition.rs`:217 on 2023-05-10 17:25_

Here, we iterate over definitions and compute visibilities as we go. We can do it in a single pass, as it's assumed that parents appear before children (this assumption is documented above, on `Definitions`).

---

_@charliermarsh reviewed on 2023-05-10 17:25_

---

_Comment by @MichaReiser on 2023-05-11 07:49_

> Note that we're not yet respecting __all__ -- that will come in a separate PR. But this PR makes it "trivial" to add that behavior.

One thing that we could do here that I haven't thought about before: Keep the logic as is but don't emit the `Diagnostics` right away. Instead, store them in a temporary `Vec` with, potentially some metadata. When we encounter an `_all__`, go through all emitted `Diagnostic`s and remove the `Diagnostic`s for the private symbols. 

This will have worse performance for projects that use `__all__` extensively but my understanding is that not-using `__all__` is the more common pattern. 

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:5398 on 2023-05-11 07:53_

Why is it no longer necessary to to check if `self.ctx.definitions` doesn't contain new entries after iterating all definitions? Or was that never necessary to begin with?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pydocstyle/rules/sections.rs`:754 on 2023-05-11 07:58_

Nit: Add `const fn is_method(&self) -> bool` method to `Definition`. 

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/analyze/visibility.rs`:141 on 2023-05-11 08:00_

Nit: Either implement as `to_visibility(&self)` on `ModuleSource` or as `Visibility::from_module_source(source: &Modulesource)`. Adding it to an object rather than having free-floating functions can help with discoverability. 

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/definition.rs`:68 on 2023-05-11 08:01_

```suggestion
#[derive(Debug, Copy, Clone)]
```

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/definition.rs`:68 on 2023-05-11 08:01_

```suggestion
#[derive(Debug, Copy, Clone)]
```

---

_@MichaReiser approved on 2023-05-11 08:05_

---

_@charliermarsh reviewed on 2023-05-11 16:57_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:5398 on 2023-05-11 16:57_

It was never necessary, but we did it for consistency.

---

_Comment by @charliermarsh on 2023-05-11 16:58_

> One thing that we could do here that I haven't thought about before: Keep the logic as is but don't emit the Diagnostics right away. Instead, store them in a temporary Vec with, potentially some metadata. When we encounter an _all__, go through all emitted Diagnostics and remove the Diagnostics for the private symbols.

This still requires most of the changes I made here, because you need to be able to retroactively check visibility up the parent tree, which our current abstractions don't allow. (We don't store parent-child relationships for definitions right now, so there'd be no way to know whether a child definition is now private.)

---

_@charliermarsh reviewed on 2023-05-11 17:05_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:5398 on 2023-05-11 17:05_

Added a docstring to clarify this.

---

_Comment by @charliermarsh on 2023-05-11 17:10_

Thanks, took all suggestion :)

---

_Merged by @charliermarsh on 2023-05-11 17:14_

---

_Closed by @charliermarsh on 2023-05-11 17:14_

---

_Branch deleted on 2023-05-11 17:14_

---
