```yaml
number: 20897
title: "[ty] add legacy namespace package support"
type: pull_request
state: merged
author: Gankra
labels:
  - ty
assignees: []
merged: true
base: main
head: gankra/legacy-namespaces
created_at: 2025-10-15T15:34:02Z
updated_at: 2025-10-17T03:16:39Z
url: https://github.com/astral-sh/ruff/pull/20897
synced_at: 2026-01-12T15:57:11Z
```

# [ty] add legacy namespace package support

---

_@Gankra_

Detect legacy namespace packages and treat them like namespace packages when looking them up as the *parent* of the module we're interested in. In all other cases treat them like a regular package.

(This PR is coauthored by @MichaReiser in a shared coding session)

Fixes https://github.com/astral-sh/ty/issues/838

---

_Comment by @github-actions[bot] on 2025-10-15 15:36_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-15 15:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Renamed from "WIP: legacy namespace package support" to "[ty] WIP: legacy namespace package support" by @AlexWaygood on 2025-10-15 16:11_

---

_Label `ty` added by @AlexWaygood on 2025-10-15 16:11_

---

_Renamed from "[ty] WIP: legacy namespace package support" to "[ty] add legacy namespace package support" by @Gankra on 2025-10-15 23:43_

---

_Marked ready for review by @Gankra on 2025-10-15 23:43_

---

_Review requested from @carljm by @Gankra on 2025-10-15 23:43_

---

_Review requested from @AlexWaygood by @Gankra on 2025-10-15 23:43_

---

_Review requested from @sharkdp by @Gankra on 2025-10-15 23:43_

---

_Review requested from @dcreager by @Gankra on 2025-10-15 23:43_

---

_Comment by @Gankra on 2025-10-15 23:44_

I'm now reasonably happy with the shape of the implementation, main place to quibble is how much elbow grease to put into the visitor that tries to find the idiom.

---

_Comment by @MichaReiser on 2025-10-16 06:26_

For added context: This makes airflow work out-of-the box (whereas it wasn't even possible to set up the import paths correctly before) and fixes about 5k unresolved import errors

---

_@MichaReiser approved on 2025-10-16 06:40_

Thanks for finishing this up. Looks good to me but I'll leave this open for a little longer in case someone else wants to take a look

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:985 on 2025-10-16 18:24_

to better distinguish the two kinds of namespace packages?

```suggestion
            // we actually want. Here, such a package should be treated as a PEP-420 ("modern") namespace package.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:1026 on 2025-10-16 18:24_

```suggestion
/// Before PEP 420 introduced implicit namespace packages, the ecosystem developed
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:1028 on 2025-10-16 18:25_

```suggestion
/// in modern codebases because they work with ancient Pythons and if it ain't broke don't, fix it.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:1040 on 2025-10-16 18:28_

```suggestion
/// * Like implicit namespace packages, multiple copies of the package may exist with different submodules,
///   and these submodules will be merged into one namespace at runtime by the interpreter
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:1025 on 2025-10-16 18:29_

this comment is fantastic, thanks for writing it up!!

---

_@AlexWaygood approved on 2025-10-16 18:31_

This looks great, thank you!! I only have a couple of minor nits about comments

---

_Merged by @Gankra on 2025-10-17 03:16_

---

_Closed by @Gankra on 2025-10-17 03:16_

---

_Branch deleted on 2025-10-17 03:16_

---
