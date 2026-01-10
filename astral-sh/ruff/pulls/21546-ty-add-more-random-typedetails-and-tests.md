```yaml
number: 21546
title: "[ty] Add more random TypeDetails and tests"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/more-click
created_at: 2025-11-20T19:17:48Z
updated_at: 2025-11-20T19:46:19Z
url: https://github.com/astral-sh/ruff/pull/21546
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Add more random TypeDetails and tests

---

_Pull request opened by @Gankra on 2025-11-20 19:17_

_No description provided._

---

_Review requested from @carljm by @Gankra on 2025-11-20 19:17_

---

_Review requested from @AlexWaygood by @Gankra on 2025-11-20 19:17_

---

_Review requested from @sharkdp by @Gankra on 2025-11-20 19:17_

---

_Review requested from @dcreager by @Gankra on 2025-11-20 19:17_

---

_Review requested from @MichaReiser by @Gankra on 2025-11-20 19:17_

---

_Label `server` added by @Gankra on 2025-11-20 19:17_

---

_Label `ty` added by @Gankra on 2025-11-20 19:17_

---

_@Gankra reviewed on 2025-11-20 19:19_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/display.rs`:675 on 2025-11-20 19:19_

This is theoretically useless because the span logic will prefer the inner `class.display_with` but this is effectively a fallback if it doesn't.

---

_Comment by @astral-sh-bot[bot] on 2025-11-20 19:19_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_@Gankra reviewed on 2025-11-20 19:20_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/display.rs`:715 on 2025-11-20 19:20_

Should this invoke the Any/Unknown special cases? (Should that logic be factored out?)

---

_Comment by @astral-sh-bot[bot] on 2025-11-20 19:21_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 43 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:702 on 2025-11-20 19:24_

I'm actually not totally sold that double-clicking on `type` in `type[int]` should take you anywhere, because `type` isn't a generic class in the same way as `list[int]` or `dict[str, str]`. Though it is true that all `type[]` types are instances of `type`

If we _do_ want it to take you somewhere, though, it should take you to `builtins.type` rather than `typing.Type`, so that would be

```suggestion
                    f.with_detail(TypeDetail::Type(KnownClass::Type.to_class_literal(self.db)))
                        .write_str("type")?;
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:710 on 2025-11-20 19:25_

same here

```suggestion
                    f.with_detail(TypeDetail::Type(KnownClass::Type.to_class_literal(self.db)))
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:719 on 2025-11-20 19:25_

and here

```suggestion
                    f.with_detail(TypeDetail::Type(KnownClass::Type.to_class_literal(self.db)))
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:715 on 2025-11-20 19:25_

> Should this invoke the Any/Unknown special cases?

I think so, yes!

> Should that logic be factored out?

Sure!

---

_@AlexWaygood approved on 2025-11-20 19:27_

Nice!

---

_@AlexWaygood reviewed on 2025-11-20 19:35_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:715 on 2025-11-20 19:35_

in fact, maybe that logic should just be moved to `Type::definition()` in `types.rs`?

---

_Merged by @Gankra on 2025-11-20 19:46_

---

_Closed by @Gankra on 2025-11-20 19:46_

---

_Branch deleted on 2025-11-20 19:46_

---
