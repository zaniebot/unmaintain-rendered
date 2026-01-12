```yaml
number: 13884
title: "[red-knot] Use track_caller for expect_ methods"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/types-expect-methods-track_caller
created_at: 2024-10-23T08:32:02Z
updated_at: 2024-10-23T10:48:21Z
url: https://github.com/astral-sh/ruff/pull/13884
synced_at: 2026-01-12T15:55:46Z
```

# [red-knot] Use track_caller for expect_ methods

---

_@sharkdp_

## Summary

A minor quality-of-life improvement: add [`#[track_caller]`](https://doc.rust-lang.org/reference/attributes/codegen.html#the-track_caller-attribute) attribute to `Type::expect_xyz()` methods such that the panic-location is reported one level higher up in the stack trace.

before: reports location inside the `Type::expect_class_literal()` method. Not very useful.
```
thread 'types::infer::tests::deferred_annotation_builtin' panicked at crates/red_knot_python_semantic/src/types.rs:304:14:
Expected a Type::ClassLiteral variant
```

after: reports location at the `Type::expect_class_literal()` call site, where the error was made.
```
thread 'types::infer::tests::deferred_annotation_builtin' panicked at crates/red_knot_python_semantic/src/types/infer.rs:4302:14:
Expected a Type::ClassLiteral variant
```

## Test Plan

Called `expect_class_literal()` on something that's not a `Type::ClassLiteral` and saw that the error was reported at the call site.

---

_Label `red-knot` added by @sharkdp on 2024-10-23 08:32_

---

_Review requested from @carljm by @sharkdp on 2024-10-23 08:32_

---

_Review requested from @MichaReiser by @sharkdp on 2024-10-23 08:32_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-10-23 08:32_

---

_Comment by @github-actions[bot] on 2024-10-23 08:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila approved on 2024-10-23 08:57_

TIL, this is great, thanks

---

_@AlexWaygood approved on 2024-10-23 09:03_

Nice! TIL as well :-D

Possibly we could also consider adding this attribute to `expression_ty`, `binding_ty` and `declaration_ty` here:

https://github.com/astral-sh/ruff/blob/2f88f849726d50de91efc48ac0148395b7231cd0/crates/red_knot_python_semantic/src/types/infer.rs#L208-L222

The most common source of panics in red-knot right now seems to be that a key is unexpectedly missing from one of these HashMaps, which results in a panic with the message "No entry found for key" and the line number pointing to the line inside the method that does the lookup. It's not a very helpful stack trace!

---

_Merged by @sharkdp on 2024-10-23 10:48_

---

_Closed by @sharkdp on 2024-10-23 10:48_

---

_Branch deleted on 2024-10-23 10:48_

---
