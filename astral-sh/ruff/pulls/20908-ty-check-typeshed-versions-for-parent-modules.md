```yaml
number: 20908
title: "[ty] Check typeshed VERSIONS for parent modules when reporting failed stdlib imports"
type: pull_request
state: merged
author: Gankra
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: gankra/old-python-version
created_at: 2025-10-16T01:34:35Z
updated_at: 2025-10-16T13:25:09Z
url: https://github.com/astral-sh/ruff/pull/20908
synced_at: 2026-01-12T15:57:12Z
```

# [ty] Check typeshed VERSIONS for parent modules when reporting failed stdlib imports

---

_@Gankra_

This is a drive-by improvement that I stumbled backwards into while looking into 

* https://github.com/astral-sh/ty/issues/296

I was writing some simple tests for "thing not in old version of stdlib" diagnostics and checked what was added in 3.14, and saw `compression.zstd` and to my surprise discovered that `import compression.zstd` and `from compression import zstd` had completely different quality diagnostics.

This is because `compression` and `compression.zstd` were *both* introduced in 3.14, and so per VERSIONS policy only an entry for `compression` was added, and so we don't actually have any definite info on `compression.zstd` and give up on producing a diagnostic. However the `from compression import zstd` form fails on looking up `compression` and we *do* have an exact match for that, so it gets a better diagnostic!

(aside: I have now learned about the VERSIONS format and I *really* wish they would just enumerate all the submodules but, oh well!)

The fix is, when handling an import failure, if we fail to find an exact match *we requery with the parent module*. In cases like `compression.zstd` this lets us at least identify that, hey, not even `compression` exists, and luckily that fixes the whole issue. In cases where the parent module and submodule were introduced at different times then we may discover that the parent module is in-range and that's fine, we don't produce the richer stdlib diagnostic.

---

_Label `ty` added by @Gankra on 2025-10-16 01:34_

---

_Review requested from @carljm by @Gankra on 2025-10-16 01:34_

---

_Label `diagnostics` added by @Gankra on 2025-10-16 01:34_

---

_Review requested from @AlexWaygood by @Gankra on 2025-10-16 01:34_

---

_Review requested from @sharkdp by @Gankra on 2025-10-16 01:34_

---

_Review requested from @dcreager by @Gankra on 2025-10-16 01:34_

---

_Comment by @github-actions[bot] on 2025-10-16 01:36_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-16 01:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
- Found 52 diagnostics
+ Found 51 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @Gankra on 2025-10-16 01:37_

(Broken into two commits so you can see the before and after on the snapshot)

---

_Comment by @Gankra on 2025-10-16 01:38_

Can't help but wonder if we could statically precompute a Complete VERSIONS map...

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:4793 on 2025-10-16 07:24_

Nit: Use `module_name.ancestors()`?

---

_@MichaReiser approved on 2025-10-16 07:25_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/diagnostics/newer_python.md`:1 on 2025-10-16 11:31_

Ah nice -- can you move these existing tests to this file so that all our tests for this feature are in the same place?

https://github.com/astral-sh/ruff/blob/5fb142374daec49d53bcc00126bf485e82c015a6/crates/ty_python_semantic/resources/mdtest/import/basic.md?plain=1#L180-L207

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:4793 on 2025-10-16 11:31_

+1

---

_@AlexWaygood approved on 2025-10-16 11:32_

Ahh thank you!!

---

_@Gankra reviewed on 2025-10-16 13:12_

---

_Review comment by @Gankra on `crates/ty_python_semantic/resources/mdtest/diagnostics/newer_python.md`:1 on 2025-10-16 13:12_

I did this but I completely misread the direction you wanted this to go so instead my new test file is deleted and I added a new test to `basic.md`

---

_@Gankra reviewed on 2025-10-16 13:12_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/infer/builder.rs`:4793 on 2025-10-16 13:12_

rip `loop` 😢 

---

_@AlexWaygood reviewed on 2025-10-16 13:16_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/diagnostics/newer_python.md`:1 on 2025-10-16 13:16_

Haha, I don't mind either way, I'd just like all the tests for the feature to be together!

---

_Merged by @Gankra on 2025-10-16 13:25_

---

_Closed by @Gankra on 2025-10-16 13:25_

---

_Branch deleted on 2025-10-16 13:25_

---
