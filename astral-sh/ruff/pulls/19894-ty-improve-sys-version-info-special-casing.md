```yaml
number: 19894
title: "[ty] Improve `sys.version_info` special casing"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/version-info-spec
created_at: 2025-08-13T13:07:56Z
updated_at: 2025-08-13T13:39:15Z
url: https://github.com/astral-sh/ruff/pull/19894
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Improve `sys.version_info` special casing

---

_Pull request opened by @AlexWaygood on 2025-08-13 13:07_

## Summary

Currently the `__getitem__` method we infer for `sys.version_info` is inconsistent with the types we infer for indexing/slicing/unpacking `sys.version_info`. This isn't really a big deal, but... it would be nice to be consistent :-)

The reason for the inconsistency is that we infer `sys.version_info` as inheriting from `tuple[int, int, int, @Todo(Type Alias), int]` due to [typeshed's definition](https://github.com/python/typeshed/blob/1e537d6216d4aef76a8c96ffac9da014ccfe7fc3/stdlib/sys/__init__.pyi#L328), but our special casing elsewhere treats it as a subtype of `tuple[Literal[3], Literal[<minor Python version>], int, Literal["alpha", "beta", ""candidate", "final"], int]`. We can resolve the inconsistency by adding some special-casing for `sys.version_info` to `ClassLiteral::explicit_bases()`.

In theory adding the special casing to `ClassLiteral::explicit_bases()` would let us remove any special casing for `sys.version_info` from `instance.rs`: we could just iterate through the MRO of `sys._version_info` until we find the tuple type in its MRO, and return the tuple spec of that tuple type. In practice, that (unsurpringly) leads to a big performance regression, however. 

I'm adding the "internal" label since I doubt this really changes any behaviour that users would realistically care about.

## Test Plan

Mdtests added


---

_Label `ty` added by @AlexWaygood on 2025-08-13 13:07_

---

_Comment by @github-actions[bot] on 2025-08-13 13:09_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-13 13:09:38.590966201 +0000
+++ new-output.txt	2025-08-13 13:09:38.658966248 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(34b8)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(154c3)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @MichaReiser on 2025-08-13 13:11_

We need @carljm to fix the remaining recursive type / generic issues. The conformance test panic is irritating ;) (that's supposed to be pep talk, but I know, I'm not very good at it)

---

_Comment by @github-actions[bot] on 2025-08-13 13:12_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `internal` added by @AlexWaygood on 2025-08-13 13:15_

---

_Marked ready for review by @AlexWaygood on 2025-08-13 13:20_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-13 13:20_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-13 13:20_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-13 13:20_

---

_@AlexWaygood reviewed on 2025-08-13 13:24_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:1167 on 2025-08-13 13:24_

It's fairly easy to add caching to this method if we revert https://github.com/astral-sh/ruff/commit/d76fd103aefd809440654a4e7a31920cd757eca6, but I still haven't been able to find any evidence yet that this would provide a significant speedup. I also haven't tried doing that as an isolated change on the codspeed runners, though; I've only experimented by running benchmarks locally.

---

_@MichaReiser reviewed on 2025-08-13 13:33_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:1353 on 2025-08-13 13:33_

Just something to be aware of. This has a risk of lock congestion if `explict_bases` is very hot because we keep interning the same `TupleType`. I don't think `explict_bases` is that hot that this is an issue here but I thought it's worth noting (and it's also something that we might just need to fix in Salsa)

---

_@carljm approved on 2025-08-13 13:36_

---

_@carljm reviewed on 2025-08-13 13:37_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:1353 on 2025-08-13 13:37_

I think this is likely not an issue here because we only intern when we call `explicit_bases` on the `VersionInfo` class specifically. And this method is cached so if we do that frequently, all but the first time in a revision wouldn't reach here anyway.

---

_Merged by @AlexWaygood on 2025-08-13 13:39_

---

_Closed by @AlexWaygood on 2025-08-13 13:39_

---

_Branch deleted on 2025-08-13 13:39_

---
