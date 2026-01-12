```yaml
number: 15437
title: "[red-knot] Add `AlwaysTruthy` and `AlwaysFalsy` to `knot_extensions`"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
assignees: []
merged: true
base: main
head: rk-knot-extensions
created_at: 2025-01-12T00:12:31Z
updated_at: 2025-01-12T17:07:08Z
url: https://github.com/astral-sh/ruff/pull/15437
synced_at: 2026-01-12T15:55:51Z
```

# [red-knot] Add `AlwaysTruthy` and `AlwaysFalsy` to `knot_extensions`

---

_@InSyncWithFoo_

## Summary

Part of #15397. There are quite a few tests that use `AlwaysTruthy`/`AlwaysFalsy`/`ClassLiteral`/`FunctionLiteral`, so it's would be convenient to have direct access to them via `knot_extensions`.

## Test Plan

Markdown tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2025-01-12 00:12_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2025-01-12 00:12_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-01-12 00:12_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2025-01-12 00:12_

---

_Comment by @github-actions[bot] on 2025-01-12 00:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:377 on 2025-01-12 07:26_

You can't use it for *unions* of literals, but I think you should be able to use `knot_extensions.TypeOf` for function literals and slice literals, in addition to class literals, as is demonstrated in the section above this one.

---

_@sharkdp reviewed on 2025-01-12 07:30_

Thank you. Adding `AlwaysTruthy` and `AlwaysFalsy` seems sensible to me. I considered adding something like `LiteralExt` in the `knot_extensions` PR, but we then decided to go with the more general `TypeOf` for now. If we add `LiteralExt`, we should probably remove `TypeOf`?

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:377 on 2025-01-12 09:55_

The concept of `TypeOf` seems better than that of `LiteralExt`. I'll leave it to you to make the call.

---

_@InSyncWithFoo reviewed on 2025-01-12 09:55_

---

_Label `red-knot` added by @AlexWaygood on 2025-01-12 10:02_

---

_@AlexWaygood reviewed on 2025-01-12 11:08_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:377 on 2025-01-12 11:08_

I think I'd also prefer to stick with `TypeOf` for now rather than adding `LiteralExt`

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:99 on 2025-01-12 12:57_

```suggestion
`AlwaysTruthy` and `AlwaysFalsy` represent the sets of all possible objects whose truthiness is always truthy
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:5415 on 2025-01-12 13:05_

I think it would be better (and certainly simpler) to be consistent with what we do for `Self` and `TypeAlias` -- i.e., infer `Unknown` if we see it subscripted. The reasoning is that we know that it's an invalid type form but we don't know what the user intended, and it may be better not to guess.

Maybe we could consider changing this, and instead infer `Self` if we see `Self[int]` etc., but I'd prefer to be consistent in our handling of all these special symbols. I.e., if we wanted to change that, we should change them all at once in the same PR, rather than being inconsistent about our treatment between different symbols.

TL;DR, I'd make this change to your PR branch:

```diff
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -5386,7 +5386,9 @@ impl<'db> TypeInferenceBuilder<'db> {
             }
             KnownInstanceType::TypingSelf
             | KnownInstanceType::TypeAlias
-            | KnownInstanceType::Unknown => {
+            | KnownInstanceType::Unknown
+            | KnownInstanceType::AlwaysFalsy
+            | KnownInstanceType::AlwaysTruthy => {
                 self.context.report_lint(
                     &INVALID_TYPE_FORM,
                     subscript.into(),
@@ -5397,22 +5399,6 @@ impl<'db> TypeInferenceBuilder<'db> {
                 );
                 Type::unknown()
             }
-            KnownInstanceType::AlwaysTruthy | KnownInstanceType::AlwaysFalsy => {
-                self.context.report_lint(
-                    &INVALID_TYPE_FORM,
-                    subscript.into(),
-                    format_args!(
-                        "Special form `{}` expected no type parameter",
-                        known_instance.repr(self.db())
-                    ),
-                );
-
-                if matches!(known_instance, KnownInstanceType::AlwaysTruthy) {
-                    Type::AlwaysTruthy
-                } else {
-                    Type::AlwaysFalsy
-                }
-            }
```

It would also be good to add a test covering this branch -- no tests failed when I applied this change locally to your PR branch

---

_@AlexWaygood reviewed on 2025-01-12 13:05_

---

_@AlexWaygood reviewed on 2025-01-12 16:30_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:122 on 2025-01-12 16:30_

this still doesn't provide test coverage for the issue we were dicussing earlier, which is "What type is inferred if a type argument is invalidly provided for one of these symbols?". To provide test coverage for that, you'd need to do something like this:

```suggestion
def f(
    a: AlwaysTruthy[int],  # error: [invalid-type-form]
    b: AlwaysFalsy[str],  # error: [invalid-type-form]
):
    reveal_type(a)  # revealed: Unknown
    reveal_type(b)  # revealed: Unknown
```

---

_@AlexWaygood approved on 2025-01-12 16:41_

---

_Renamed from "[red-knot] Extend `knot_extensions` with a few more symbols" to "[red-knot] Add `AlwaysTruthy` and `AlwaysFalsy` to `knot_extensions`" by @InSyncWithFoo on 2025-01-12 16:57_

---

_Merged by @AlexWaygood on 2025-01-12 17:00_

---

_Closed by @AlexWaygood on 2025-01-12 17:00_

---

_Branch deleted on 2025-01-12 17:07_

---
