```yaml
number: 16510
title: "[red-knot] support empty TypeInference with fallback type"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/fallbackty
created_at: 2025-03-05T00:14:31Z
updated_at: 2025-03-05T17:12:28Z
url: https://github.com/astral-sh/ruff/pull/16510
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] support empty TypeInference with fallback type

---

_Pull request opened by @carljm on 2025-03-05 00:14_

This is split out of https://github.com/astral-sh/ruff/pull/14029, to reduce the size of that PR, and to validate that this "fallback type" support in `TypeInference` doesn't come with a performance cost. It also improves the reliability and debuggability of our current (temporary) cycle handling.

In order to recover from a cycle, we have to be able to construct a "default" `TypeInference` where all expressions and definitions have some "default" type. In our current cycle handling, this "default" type is just unknown or a todo type. With fixpoint iteration, the "default" type will be `Type::Never`, which is the "bottom" type that fixpoint iteration starts from.

Since it would be costly (both in space and time) to actually enumerate all expressions and definitions in a scope, just to insert the same default type for all of them, instead we add an optional "missing type" fallback to `TypeInference`, which (if set) is the fallback type for any expression or definition which doesn't have an explicit type set.

With this change, cycles can no longer result in the dreaded "Missing key" errors looking up the type of some expression.

---

_Marked ready for review by @carljm on 2025-03-05 00:33_

---

_Review requested from @MichaReiser by @carljm on 2025-03-05 00:33_

---

_Review requested from @AlexWaygood by @carljm on 2025-03-05 00:33_

---

_Review requested from @sharkdp by @carljm on 2025-03-05 00:33_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-05 06:37_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:303 on 2025-03-05 07:54_

Should `try_expression_type` also have the fallback handling because `Never` is the expression's type while in a cycle. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:305 on 2025-03-05 07:56_

```suggestion
            self.missing_ty
                .expect("expression should belong to this type inference context and `TypeInferenceBuilder` should have inferred a type for it")
```

https://doc.rust-lang.org/std/result/enum.Result.html#recommended-message-style

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:318 on 2025-03-05 07:57_

Nit: I'm pretty sure it results in the same generated LLVM code but you could avoid a function call in the *good* path this way

```suggestion
        self.bindings.get(&definition).copied().or(self.missing_ty).unwrap_or_else(|| {
            panic!("missing binding type and no missing_ty")
        })
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:285 on 2025-03-05 07:59_

We started to use `type` over `ty`
```suggestion
    missing_type: Option<Type<'db>>,
```

I also suggest to use a more specific name than `missing_ty` that tells a reader more about why it is needed: `cycle_fallback_type`. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:289 on 2025-03-05 08:00_

NIT: I've a small preference to create a dedicated constructor method, e.g. `TypeInference::cycle_fallback(scope: ScopeId, type: Type)` that enforces a more strict API and may also allow us to have a stricter visibility (private?)


---

_@MichaReiser approved on 2025-03-05 08:01_

---

_@carljm reviewed on 2025-03-05 16:15_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:303 on 2025-03-05 16:15_

Yeah, not sure it will ever matter in practice but that's probably better.

---

_Merged by @carljm on 2025-03-05 17:12_

---

_Closed by @carljm on 2025-03-05 17:12_

---

_Branch deleted on 2025-03-05 17:12_

---
