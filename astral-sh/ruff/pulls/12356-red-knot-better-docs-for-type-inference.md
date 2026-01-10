```yaml
number: 12356
title: "[red-knot] better docs for type inference"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/infer-docs
created_at: 2024-07-17T03:04:46Z
updated_at: 2024-07-17T20:37:00Z
url: https://github.com/astral-sh/ruff/pull/12356
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] better docs for type inference

---

_Pull request opened by @carljm on 2024-07-17 03:04_

Add some docs for how type inference works.

Also a couple minor code changes to rearrange or rename for better clarity.

---

_Review requested from @MichaReiser by @carljm on 2024-07-17 03:04_

---

_Review requested from @AlexWaygood by @carljm on 2024-07-17 03:04_

---

_Comment by @github-actions[bot] on 2024-07-17 03:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @MichaReiser on 2024-07-17 06:13_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:5 on 2024-07-17 06:14_

I'm not sure if it's worth mentioning. I'll leave it up to you. But scope level inference is also need for the expression's `HasTy` implementation

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:141 on 2024-07-17 06:16_

```suggestion
/// The `finish` method calls [`infer_region`](TypeInferenceBuilder::infer_region), which delegates to one of `infer_region_scope`,
```

Or is this not possible because the method is `pub(crate)`?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:151 on 2024-07-17 06:16_

```suggestion
/// the semantic index and call the [`infer_definition_types`] query on it, which creates another
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:152 on 2024-07-17 06:17_

```suggestion
/// [`TypeInferenceBuilder`] just for that definition, and we merge the returned [`TypeInference`]
```

Sorry for being pedantic. I don't find it that important that these are links. My main motivation is that some IDEs are capable of updating docs when renaming a symbol (and `cargo doc` will fail if we missed renaming a reference in the documentation)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:164 on 2024-07-17 06:18_

```suggestion
/// [`Definition`] for that function in the semantic index and calls [`infer_definition_types`] on
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:165 on 2024-07-17 06:19_

```suggestion
/// it, which will create a new [`TypeInferenceBuilder`] with `InferenceRegion::Definition`, and in
```

---

_@MichaReiser approved on 2024-07-17 06:19_

Thanks. That's very helpful documentation

---

_Comment by @carljm on 2024-07-17 06:52_

Thanks, will look at adding these links tomorrow.

---

_@carljm reviewed on 2024-07-17 18:34_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:141 on 2024-07-17 18:34_

Seems to work fine. I learned a bit about rustdoc and discovered that in order to get it to actually render or even validate these docs I need `cargo doc -p red_knot_python_semantic --document-private-items` (the latter flag needed because this entire `infer` module is private).

This means that CI currently won't catch any warnings in these docs, because it doesn't build with `--document-private-items`. If we are writing these docs mostly for our own maintenance reasons anyway, maybe we should change that?

---

_@carljm reviewed on 2024-07-17 19:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:141 on 2024-07-17 19:00_

I opened https://github.com/astral-sh/ruff/issues/12372 because it feels odd to be caring about rustdoc syntax in doc comments that our CI currently doesn't validate at all.

---

_@carljm reviewed on 2024-07-17 19:14_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:151 on 2024-07-17 19:14_

FWIW rustdoc requires this to be ``[`infer_definition_types()`]`` (with the parens), because Salsa's macros create both a struct and a function by this name, and the link must disambiguate.

---

_@carljm reviewed on 2024-07-17 19:17_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:152 on 2024-07-17 19:17_

> cargo doc will fail if we missed renaming a reference in the documentation

That seems like a good enough motivation to me! But we don't get that benefit on any doc comment on a private item (and this entire module is private), unless we fix https://github.com/astral-sh/ruff/issues/12372 first.

---

_Merged by @carljm on 2024-07-17 20:36_

---

_Closed by @carljm on 2024-07-17 20:36_

---

_Branch deleted on 2024-07-17 20:37_

---
