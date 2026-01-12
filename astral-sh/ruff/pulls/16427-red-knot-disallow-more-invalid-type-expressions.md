```yaml
number: 16427
title: "[red-knot] Disallow more invalid type expressions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/reject-bad-type-exprs
created_at: 2025-02-27T23:47:08Z
updated_at: 2025-02-28T10:04:32Z
url: https://github.com/astral-sh/ruff/pull/16427
synced_at: 2026-01-12T15:55:54Z
```

# [red-knot] Disallow more invalid type expressions

---

_@AlexWaygood_

## Summary

As part of working on https://github.com/astral-sh/ruff/issues/16302, I'm auditing all uses of `Type::to_instance()`. I noticed that one use in `Type::in_type_expression` has a pretty clear bug, in that it treats `Type::SubclassOf()` the same as `Type::ClassLiteral`. That's incorrect: an object can only have a `type[]` type if it's a dynamic variable of some kind, and we should reject such variables in type expressions. The practical effect of this is that on `main`, we have this incorrect behaviour, where we _should_ be reject `y`'s type annotation altogether, but instead we accept it:

```py
def _(x: type[int]):
    def f(y: x):
        reveal_type(y)  # revealed: int
```

This PR fixes the bug by making `Type::in_type_expression()` exhaustive over all `Type` variants and rejecting most of them when they appear in type expressions. (The method is not yet exhaustive over all `KnownInstanceType` variants; some of these would require special error messages, and it seemed out of the scope of this bugfix PR.)

## Test Plan

Added mdtests.


---

_Label `red-knot` added by @AlexWaygood on 2025-02-27 23:47_

---

_Review requested from @carljm by @AlexWaygood on 2025-02-27 23:47_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-02-27 23:47_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-02-27 23:47_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/unsupported_special_forms.md`:21 on 2025-02-28 00:20_

These assertions are failing in the release build, I think because `TODO_METADATA_REGEX` in the mdtests matcher can't handle a close parenthesis in the Todo message. Might not be easy to fix that in mdtest (a greedy match could fail if there are multiple Todo types in a union). Simplest fix is to remove the parens from this todo message.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2869 on 2025-02-28 00:26_

I'm not sure about this "dynamic variable" naming. I'd rather not introduce that concept/wording without a clear principled definition of what constitutes a "dynamic variable" and how we would distinguish one from a simple implicit type alias.

What this PR actually implements is not any logic at all related to what is a "dynamic variable", just logic about what types are valid in annotations. So we have no idea how "dynamic" the variable is or isn't, just that it was of a type that isn't valid in a type expression.

(For example, an unconditional global `x = 1` is no more or less a "dynamic variable" than `x = int` is, but the former would raise this error if used in an annotation, while the latter is a valid implicit type alias. We aren't making any distinctions here based on "how dynamic", just based on the type.)
```suggestion
    /// Some types are not allowed in type expressions
    InvalidType(Type<'db>),
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2896 on 2025-02-28 00:27_

```suggestion
                        "Variable of type `{ty}` is not allowed in type expressions",
```

---

_@carljm approved on 2025-02-28 00:28_

Nice!

---

_Merged by @AlexWaygood on 2025-02-28 10:04_

---

_Closed by @AlexWaygood on 2025-02-28 10:04_

---

_Branch deleted on 2025-02-28 10:04_

---
