```yaml
number: 19892
title: "[ty] Reduce allocations in `tuple.rs`"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - performance
  - ty
assignees: []
draft: true
base: main
head: alex/homogeneous-tuple-option
created_at: 2025-08-13T11:24:11Z
updated_at: 2025-08-13T11:41:46Z
url: https://github.com/astral-sh/ruff/pull/19892
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Reduce allocations in `tuple.rs`

---

_Pull request opened by @AlexWaygood on 2025-08-13 11:24_

## Summary

The vast majority of real-world variable-length tuples are "pure homogeneous" tuples with no prefix and no suffix. We can optimize for the common case by using `Option<Box<[T]>>` for these fields rather than `Box<[T]>`, which in many cases allows us to avoid allocating.

An alternative approach here would be to use a different `Tuple` variant entirely for pure-homogeneous tuples that have no prefix and no suffix: rather than just `Tuple::FixedLength` and `Tuple::VariableLength`, we could have `Tuple::Heterogeneous`, `Tuple::Homogeneous` and `Tuple::Mixed`. I do like that the current design is quite expressive of the fact that a "pure homogeneous" tuple really has exactly the same semantics as a "mixed tuple" with a 0-length prefix and a 0-length suffix, however, so that's an argument for keeping homogeneous tuples and mixed tuples together in one `Tuple` variant.

## Test Plan

Existing tests pass. The mypy primer report is clean.


---

_Label `ty` added by @AlexWaygood on 2025-08-13 11:24_

---

_Label `performance` added by @AlexWaygood on 2025-08-13 11:24_

---

_Comment by @github-actions[bot] on 2025-08-13 11:26_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-13 11:25:55.667497923 +0000
+++ new-output.txt	2025-08-13 11:25:55.735498362 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(120da)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(30a0)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-13 11:28_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@MichaReiser reviewed on 2025-08-13 11:32_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/tuple.rs`:532 on 2025-08-13 11:32_

I don't think `Box<[T]>` allocates. I'm not sure what an empty box slice uses as start-pointer but they can use any value for as long as they don't dereference it when `length` is zero

---

_@AlexWaygood reviewed on 2025-08-13 11:35_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:532 on 2025-08-13 11:35_

Hmm, this is maybe poorly worded. My point is that are lots of places on `main` where we call `.collect()` on an iterator to create a `Box<[T]>` for the `prefix` or `suffix`, but we don't really need to if we know ahead of time that the iterator is 0-length (which is by far the most common case).

---

_Comment by @AlexWaygood on 2025-08-13 11:37_

No impact on Codspeed benchmarks `¯\_(ツ)_/¯`

---

_Closed by @AlexWaygood on 2025-08-13 11:37_

---

_Branch deleted on 2025-08-13 11:37_

---

_@MichaReiser reviewed on 2025-08-13 11:39_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/tuple.rs`:532 on 2025-08-13 11:39_

I think this optimization (I'm not sure if it's something we'll see in benchmarks) wouldn't require an `Option<Box<[T]>>`, you could just call `Box::default()` if the iterator is empty. 

However, I'd expect Rust to already short-circuit for you if `sufix` and `prefix` are known to be empty because Rust can use specialization internally (they can have a custom `FromIter` implementation when `I` implements `ExactSizeIterator`).

So I suspect that Rust already does all of that for you

---

_@AlexWaygood reviewed on 2025-08-13 11:41_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:532 on 2025-08-13 11:41_

Makes sense. Well, it was just an experiment :-)

---
