```yaml
number: 16326
title: "[red-knot] Add a test to ensure that `KnownClass::try_from_file_and_name()` is kept up to date"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/redknot-knownclass-enum
created_at: 2025-02-23T21:49:28Z
updated_at: 2025-02-24T12:15:41Z
url: https://github.com/astral-sh/ruff/pull/16326
synced_at: 2026-01-12T15:55:54Z
```

# [red-knot] Add a test to ensure that `KnownClass::try_from_file_and_name()` is kept up to date

---

_@AlexWaygood_

## Summary

We keep on adding new variants to the `KnownClass` enum but forgetting to update this method, because the Rust compiler only enforces exhaustiveness over enums when the enum variants appear on the left-hand side of the `match` statement:

https://github.com/astral-sh/ruff/blob/b312b53c2eafd97a084f9f2961a99fbfa257f21a/crates/red_knot_python_semantic/src/types.rs#L3183-L3231

That leads to subtle and confusing bugs that can be hard to debug, and that sometimes go unnoticed for several weeks.

This PR adds a test that will fail if we add a variant to the `KnownClass` enum and forget to add a case to `KnownClass::try_from_file_and_name()`. It also adds tests for the similar methods on the `KnownModule` and `KnownFunction` enums.

## How it works

~~`strum` and `strum_macros` are added as _test-only_ dependencies for red-knot (they are added to the `dev-dependencies` table in the `red_knot_python_semantic` crate's `Cargo.toml`). If the `test` feature is enabled, we derive the `EnumIter` trait for the `KnownClass` enum. In the tests for `KnownClass`, we then use the generated `iter()` method to iterate over all `KnownClass` variants and verify that each variant has its own branch in the `KnownClass::try_from_file_and_name()` function.~~

Following review, this has been changed:
- `strum` and `strum_macros` are now just regular dependencies for red-knot, in production as well as if the `test` feature is enabled.
- For `KnownFunction` and `KnownModule`, we use the `EnumString` macro from `strum_macros` to simplify the deserialization constructors and ensure that they are exhaustive over the right-hand side. (This unfortunately doesn't work for `KnownClass` because of the fact that the `EllipsisType` branch deserializes dynamically depending on the target Python version.)
- For all three enums, we add tests to ensure that the deserialization functions are both exhaustive and correct. This is done using strum's `EnumIter` macro.

## Limitations

Unfortunately, this approach doesn't work for the `KnownInstanceType` enum. `KnownInstanceType` is generic over a lifetime, and the `EnumIter` trait cannot be derived for enums generic over a lifetime. I therefore haven't touched that enum in this PR.

This approach also necessitated some small refactoring of the `KnownFunction` enum. `EnumIter` and `EnumString` couldn't be derived for the enum as-is because of the fact that the `ConstraintFunction` variant wrapped inner data. I solved this by replacing the `ConstraintFunction` variant (which wrapped an inner enum) with two variants (`KnownFunction::IsSubclass` and `KnownFunction::IsInstance`), and slightly reworking the `KnownFunction::constraint_function()` method (which is renamed to `KnownFunction::into_constraint_function()`)

---

_Label `testing` added by @AlexWaygood on 2025-02-23 21:49_

---

_Label `red-knot` added by @AlexWaygood on 2025-02-23 21:49_

---

_Review requested from @carljm by @AlexWaygood on 2025-02-23 21:49_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-02-23 21:49_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-02-23 21:49_

---

_Comment by @github-actions[bot] on 2025-02-23 21:58_

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

_@MichaReiser reviewed on 2025-02-24 07:19_

Could we use `strum::EnumString` https://github.com/Peternator7/strum/wiki/Derive-EnumString if we're adding a `strum` dependency anyway?




---

_Comment by @AlexWaygood on 2025-02-24 07:52_

> Could we use `strum::EnumString` https://github.com/Peternator7/strum/wiki/Derive-EnumString if we're adding a `strum` dependency anyway?

This PR currently only adds strum as a dev-dependency. But I can make that change if we're happy to have the extra dependency in production as well.

---

_Comment by @sharkdp on 2025-02-24 07:57_

> Could we use `strum::EnumString` https://github.com/Peternator7/strum/wiki/Derive-EnumString if we're adding a `strum` dependency anyway?

I also played around a bit with a solution to this TODO on the weekend. One thing that makes this more complicated are branches like this, where we have additional logic in the enum->string function:
```rs
            Self::EllipsisType => {
                // Exposed as `types.EllipsisType` on Python >=3.10;
                // backported as `builtins.ellipsis` by typeshed on Python <=3.9
                if Program::get(db).python_version(db) >= PythonVersion::PY310 {
                    "EllipsisType"
                } else {
                    "ellipsis"
                }
            }
```

---

_Comment by @MichaReiser on 2025-02-24 07:58_

> This PR currently only adds strum as a dev-dependency. But I can make that change if we're happy to have the extra dependency in production as well.

There shouldn't really be any difference. `strum` is 99% macros and traits only

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/narrow.rs`:92 on 2025-02-24 11:38_

Hmm, I find this now much harder to read (I can see that it now matches the builtins name but I sort of prefer the old spelling)

---

_@MichaReiser approved on 2025-02-24 11:38_

This is great. Thank you (and sorry for the merge conflict :()

---

_@AlexWaygood reviewed on 2025-02-24 11:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:92 on 2025-02-24 11:42_

Bah! I'll switch it back and add a `#[strum(serialize = "issubclass")]` attribute to the variant 😄

---

_Merged by @AlexWaygood on 2025-02-24 12:14_

---

_Closed by @AlexWaygood on 2025-02-24 12:14_

---

_Branch deleted on 2025-02-24 12:14_

---
