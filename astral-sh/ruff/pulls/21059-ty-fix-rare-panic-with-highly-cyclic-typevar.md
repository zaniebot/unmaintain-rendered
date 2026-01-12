```yaml
number: 21059
title: "[ty] Fix rare panic with highly cyclic `TypeVar` definitions"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: micha/update-salsa-iteration-count
created_at: 2025-10-24T11:11:18Z
updated_at: 2025-10-24T16:30:55Z
url: https://github.com/astral-sh/ruff/pull/21059
synced_at: 2026-01-12T15:57:15Z
```

# [ty] Fix rare panic with highly cyclic `TypeVar` definitions

---

_@MichaReiser_

Updates salsa to include a fix for https://github.com/salsa-rs/salsa/pull/1014


## Testing

I tested that ty now "successfully" and consistently crashes with "too many cycle iterations" instead of an internal salsa panic (mismatching iteration counts). 

Since I didn't manage to write a regression test in Salsa, I wrote one in ty instead. I extended mdtest to support tests that expect panics (needed because the example now panics with *too many iterations*) and added a minified version of steam.py. 

This mdtest might also be helpful when debugging why steam.py still panics.

---

_Label `bug` added by @MichaReiser on 2025-10-24 11:11_

---

_Label `ty` added by @MichaReiser on 2025-10-24 11:11_

---

_Label `bug` added by @MichaReiser on 2025-10-24 11:11_

---

_Label `ty` added by @MichaReiser on 2025-10-24 11:11_

---

_Comment by @github-actions[bot] on 2025-10-24 11:13_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-24 16:13:42.515954648 +0000
+++ new-output.txt	2025-10-24 16:13:45.600972650 +0000
@@ -1,4 +1,4 @@
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/d38145c/src/function/execute.rs:417:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(17843)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/25b3ef1/src/function/execute.rs:419:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(17843)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-10-24 11:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-10-24 11:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @MichaReiser on 2025-10-24 13:08_

---

_Review requested from @carljm by @MichaReiser on 2025-10-24 13:08_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-10-24 13:08_

---

_Review requested from @sharkdp by @MichaReiser on 2025-10-24 13:08_

---

_Review requested from @dcreager by @MichaReiser on 2025-10-24 13:08_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/regression/1377_iteration_count_mismatch.md`:8 on 2025-10-24 13:11_

haha, nice feature! This might be useful to track the remaining panics in https://github.com/astral-sh/ty/issues/256

---

_Review comment by @AlexWaygood on `crates/ty_test/src/lib.rs`:2 on 2025-10-24 13:12_

is this necessary?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/regression/1377_iteration_count_mismatch.md`:1 on 2025-10-24 13:24_

Playing around with this locally, I can make this test quite a lot more minimal while still triggerin the `too many cycle iterations` panic. But I'm guessing you can no longer repro the Salsa bug on the more minimal versions of this test?

---

_@AlexWaygood approved on 2025-10-24 13:24_

Nice!

---

_@MichaReiser reviewed on 2025-10-24 13:27_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/regression/1377_iteration_count_mismatch.md`:1 on 2025-10-24 13:27_

Yes, removing any line immediately goes from the panic that I'm fixing in salsa to `too many cycle iterations`. 

I suspect that it might be possible to inline some `TypeVar`s (reduce some indirection) and still get the same outcome but I decided that it's already pretty good and not to spend more time (2h!) on minimizing

---

_@MichaReiser reviewed on 2025-10-24 13:27_

---

_Review comment by @MichaReiser on `crates/ty_test/src/lib.rs`:2 on 2025-10-24 13:27_

na. Thanks

---

_@AlexWaygood reviewed on 2025-10-24 13:36_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/regression/1377_iteration_count_mismatch.md`:1 on 2025-10-24 13:36_

oh yeah, for sure. This is fine as-is

---

_Merged by @MichaReiser on 2025-10-24 16:30_

---

_Closed by @MichaReiser on 2025-10-24 16:30_

---

_Branch deleted on 2025-10-24 16:30_

---
