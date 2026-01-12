```yaml
number: 14284
title: "[red-knot] Review remaining 'possibly unbound' call sites"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/review-possibly_unbound-cases
created_at: 2024-11-11T15:39:43Z
updated_at: 2024-11-11T19:48:51Z
url: https://github.com/astral-sh/ruff/pull/14284
synced_at: 2026-01-12T15:55:47Z
```

# [red-knot] Review remaining 'possibly unbound' call sites

---

_@sharkdp_

## Summary

- Emit diagnostics when looking up (possibly) unbound attributes
- More explicit test assertions for unbound symbols
- Review remaining call sites of `Symbol::ignore_possibly_unbound`. Most of them are something like `builtins_symbol(self.db, "Ellipsis").ignore_possibly_unbound().unwrap_or(Type::Unknown)` which look okay to me, unless we want to emit additional diagnostics. There is one additional case in enum literal handling, which has a TODO comment anyway.

part of #14022

## Test Plan

New MD tests for (possibly) unbound attributes.

---

_Label `red-knot` added by @sharkdp on 2024-11-11 15:39_

---

_Review requested from @carljm by @sharkdp on 2024-11-11 15:39_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-11 15:39_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-11 15:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4754 on 2024-11-11 15:53_

This function also has a bunch of assertions and other places where it could panic; maybe it's worth adding `#[track_caller]` to this as well?

```suggestion
    #[track_caller]
    fn get_symbol<'db>(
```

---

_Comment by @github-actions[bot] on 2024-11-11 15:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:5468 on 2024-11-11 15:54_

```suggestion
        // Iterating over an unbound iterable yields `Unknown`:
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2799 on 2024-11-11 15:55_

I'd go for a slightly shorter error message here:

```suggestion
                            "Attribute `{}` on type `{}` is possibly unbound",
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2792 on 2024-11-11 15:56_

while we're here, we can also clean this up (the method just takes `&str` these days):

```suggestion
        match value_ty.member(self.db, &attr.id) {
```

---

_@AlexWaygood approved on 2024-11-11 15:57_

---

_@carljm approved on 2024-11-11 17:35_

Agree with Alex's comments, but this looks great!

---

_Merged by @sharkdp on 2024-11-11 19:48_

---

_Closed by @sharkdp on 2024-11-11 19:48_

---

_Branch deleted on 2024-11-11 19:48_

---
