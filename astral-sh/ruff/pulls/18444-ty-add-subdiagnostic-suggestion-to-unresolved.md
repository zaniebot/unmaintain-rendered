```yaml
number: 18444
title: "[ty] Add subdiagnostic suggestion to `unresolved-reference` diagnostic when variable exists on `self`"
type: pull_request
state: merged
author: lipefree
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: suggest-self
created_at: 2025-06-03T16:21:46Z
updated_at: 2025-06-04T15:13:50Z
url: https://github.com/astral-sh/ruff/pull/18444
synced_at: 2026-01-12T15:56:19Z
```

# [ty] Add subdiagnostic suggestion to `unresolved-reference` diagnostic when variable exists on `self`

---

_@lipefree_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Implements https://github.com/astral-sh/ty/issues/502.

In the following example:
```py
class Foo:
    x: int

    def method(self):
        y = x
```
The user may intended to use `y = self.x` in `method`. 

This is now added as a subdiagnostic in the following form : 

`info: An attribute with the same name as 'x' is defined, consider using 'self.x'`

Currently the following will not raise the subdiagnostic which is what I am working on to close this PR: 
```py
class Foo:
    x: int = 0

    def method(self):
        y = x
```

## Test Plan

<!-- How was it tested? -->
I will add mdtests with snapshots to capture the subdiagnostic (I just have to find the right file).

---

_Comment by @github-actions[bot] on 2025-06-03 16:24_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `ty` added by @ntBre on 2025-06-03 16:40_

---

_Label `diagnostics` added by @AlexWaygood on 2025-06-04 09:46_

---

_Marked ready for review by @lipefree on 2025-06-04 14:00_

---

_Review requested from @carljm by @lipefree on 2025-06-04 14:00_

---

_Review requested from @AlexWaygood by @lipefree on 2025-06-04 14:00_

---

_Review requested from @sharkdp by @lipefree on 2025-06-04 14:00_

---

_Review requested from @dcreager by @lipefree on 2025-06-04 14:00_

---

_Converted to draft by @lipefree on 2025-06-04 14:11_

---

_Comment by @lipefree on 2025-06-04 14:12_

Sorry I did not see the panic found by the fuzzing, I will work on it

---

_Comment by @AlexWaygood on 2025-06-04 14:17_

if you make this change to the `TypeInferenceBuilder::class_context_of_current_method()` method on your PR branch, it will fix the fuzzing failure:

```diff
diff --git a/crates/ty_python_semantic/src/types/infer.rs b/crates/ty_python_semantic/src/types/infer.rs
index abb6f91206..8b627ac33c 100644
--- a/crates/ty_python_semantic/src/types/infer.rs
+++ b/crates/ty_python_semantic/src/types/infer.rs
@@ -1714,6 +1714,10 @@ impl<'db> TypeInferenceBuilder<'db> {
     fn class_context_of_current_method(&self) -> Option<ClassLiteral<'db>> {
         let current_scope_id = self.scope().file_scope_id(self.db());
         let current_scope = self.index.scope(current_scope_id);
+        if current_scope.kind() != ScopeKind::Function {
+            return None;
+        }
+
         let parent_scope_id = current_scope.parent()?;
         let parent_scope = self.index.scope(parent_scope_id);
```

It's a pre-existing bug that wasn't caused by your PR -- it's just been exposed by your PR!

---

_Comment by @lipefree on 2025-06-04 14:25_

> if you make this change to your PR branch, it will fix the fuzzing failure:
> 
> ```diff
> diff --git a/crates/ty_python_semantic/src/types/infer.rs b/crates/ty_python_semantic/src/types/infer.rs
> index abb6f91206..8b627ac33c 100644
> --- a/crates/ty_python_semantic/src/types/infer.rs
> +++ b/crates/ty_python_semantic/src/types/infer.rs
> @@ -1714,6 +1714,10 @@ impl<'db> TypeInferenceBuilder<'db> {
>      fn class_context_of_current_method(&self) -> Option<ClassLiteral<'db>> {
>          let current_scope_id = self.scope().file_scope_id(self.db());
>          let current_scope = self.index.scope(current_scope_id);
> +        if current_scope.kind() != ScopeKind::Function {
> +            return None;
> +        }
> +
>          let parent_scope_id = current_scope.parent()?;
>          let parent_scope = self.index.scope(parent_scope_id);
> ```
> 
> It's a pre-existing bug that wasn't caused by your PR -- it's just been exposed by your PR!

Thank you so much @AlexWaygood, I was going for a full investigation but on my code only ! You are saving me so much time here ! I will then add it to my PR!

---

_Marked ready for review by @lipefree on 2025-06-04 14:46_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/diagnostic.rs`:1804 on 2025-06-04 14:54_

nit: this seems a little more verbose than necessary
```suggestion
            "An attribute `{id}` is available, consider using `self.{id}`"
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:5900 on 2025-06-04 14:55_

```suggestion
                        let attribute_exists = if let Some(class) = self.class_context_of_current_method() {
```

---

_@carljm approved on 2025-06-04 14:56_

Looks great, thank you! Couple of minor nits.

---

_Merged by @carljm on 2025-06-04 15:13_

---

_Closed by @carljm on 2025-06-04 15:13_

---
