```yaml
number: 13117
title: "red-knot: infer multiplication for strings and integers"
type: pull_request
state: merged
author: chriskrycho
labels:
  - ty
assignees: []
merged: true
base: main
head: rk-string-times-integer
created_at: 2024-08-26T22:00:00Z
updated_at: 2024-09-03T07:33:58Z
url: https://github.com/astral-sh/ruff/pull/13117
synced_at: 2026-01-12T15:55:43Z
```

# red-knot: infer multiplication for strings and integers

---

_@chriskrycho_

## Summary

The resulting type when multiplying a string literal by an integer literal is one of two types:

- `StringLiteral`, in the case where it is a reasonably small resulting string (arbitrarily bounded here to 4096 bytes, roughly a page on many operating systems), including the fully expanded string.
- `LiteralString`, matching Pyright etc., for strings larger than that.

Additionally:

- Switch to using `Box<str>` instead of `String` for the internal value of `StringLiteral`, saving some non-trivial byte overhead (and keeping the total number of allocations the same).
- Be clearer and more accurate about which types we ought to defer to in `StringLiteral` and `LiteralString` member lookup.

## Test Plan

Added a test case covering multiplication times integers: positive,  negative, zero, and in and out of bounds.

---

_Review requested from @carljm by @chriskrycho on 2024-08-26 22:00_

---

_Review requested from @MichaReiser by @chriskrycho on 2024-08-26 22:00_

---

_Review requested from @AlexWaygood by @chriskrycho on 2024-08-26 22:00_

---

_Comment by @github-actions[bot] on 2024-08-26 22:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:190 on 2024-08-26 22:48_

Suggested edit to the doc comment:

```suggestion
    /// A string known to originate only from literal values, but whose value is not known (unlike StringLiteral above).
```

Also suggest placing this next to StringLiteral.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:289 on 2024-08-26 22:51_

Let's actually separate these cases and take into account @AlexWaygood's comment at https://github.com/astral-sh/ruff/pull/13113#discussion_r1731661129 -- that the `member` implementation for `StringLiteral` should in many cases be `LiteralString`, not based on the type annotations on `builtins.str` in typeshed.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:224 on 2024-08-26 22:52_

Nit: for this name to be accurate, we should also enforce it in `infer_string_literal_expression`, right?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2412 on 2024-08-26 22:54_

Seems like in practice multiplying by a negative always results in empty string, so we could go with `Literal[""]` here instead of `LiteralString`?

---

_@carljm approved on 2024-08-26 22:55_

Looks good! A few suggested changes, all minor.

I'd also like to follow up on https://github.com/astral-sh/ruff/pull/13113#discussion_r1731661713, either in this PR or another one.

---

_@AlexWaygood reviewed on 2024-08-27 10:15_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1789 on 2024-08-27 10:15_

Is the multiplication here safe from overflow? Considering https://github.com/astral-sh/ruff/pull/13117/files#r1731915249 as well, I wonder if something like this might be better for this branch:

```rs
            (Type::StringLiteral(s), Type::IntLiteral(n), ast::Operator::Mult)
            | (Type::IntLiteral(n), Type::StringLiteral(s), ast::Operator::Mult) => {
                if n < 1 {
                    Type::StringLiteral(StringLiteralType::new(self.db, String::new()))
                } else if let Ok(n) = usize::try_from(n) {
                    if n.checked_mul(s.value(self.db).len())
                        .is_some_and(|new_length| new_length <= Self::MAX_STRING_LITERAL_SIZE)
                    {
                        let new_literal = s.value(self.db).repeat(n);
                        Type::StringLiteral(StringLiteralType::new(self.db, new_literal))
                    } else {
                        Type::LiteralString
                    }
                } else {
                    // This throws away the conversion error because we don't
                    // care: it just means the value was too large.
                    Type::LiteralString
                }
            }
```

---

_@AlexWaygood reviewed on 2024-08-27 10:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:289 on 2024-08-27 10:18_

To be clear here -- typeshed has lots of overloads in its `str` stub for `LiteralString`, so we should be able to use the typeshed `str` stub even for `LiteralString`. But what we'll need to do is make sure that we process the member access as e.g. `str.capitalize(<instance of LiteralString>)` rather than `str.capitalize(<instance of str>)`, so that we correctly make use of the first overload in places like <https://github.com/python/typeshed/blob/f58dac1d62d33bb2255b762783d06463c40f5065/stdlib/builtins.pyi#L440-L443> rather than the second one 

---

_@chriskrycho reviewed on 2024-08-27 14:34_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types/infer.rs`:1789 on 2024-08-27 14:34_

Ah, good call; I thought about overflow on the `try_from` but not the multiplication. Will update!

---

_@AlexWaygood approved on 2024-08-27 15:33_

Nice, thanks!

---

_Merged by @carljm on 2024-08-27 16:00_

---

_Closed by @carljm on 2024-08-27 16:00_

---

_Branch deleted on 2024-08-27 16:22_

---

_Label `red-knot` added by @carljm on 2024-08-27 23:44_

---

_@MichaReiser reviewed on 2024-09-03 07:26_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1255 on 2024-09-03 07:26_

This seems problematic because we now have no way of knowing whether we omitted a string literal type from inference or if it is an empty string in the source code. 

---

_@MichaReiser reviewed on 2024-09-03 07:27_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1785 on 2024-09-03 07:27_

Can't this result in string literals that are larger than `MAX_STRING_LITERAL_SIZE`?

---

_@MichaReiser reviewed on 2024-09-03 07:31_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1255 on 2024-09-03 07:31_

For example in `a + b` we concatenated the strings. If `a > MAX_STRING_LITERAL_SIZE` then the concatenated string is just `b` but that's not the correct literal value. 

I also think it's relevant when displaying the type. We should display a "truncated" string type as `Literal[str]` and not `Literal[""]`

Overall it seems valuable to me knowing whether we degraded the string literal from a specific string to any string literal.

---

_@AlexWaygood reviewed on 2024-09-03 07:33_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1255 on 2024-09-03 07:33_

Yeah sorry I misread; I deleted my comment here. But please see my comment https://github.com/astral-sh/ruff/issues/13224#issuecomment-2325801446 on your issue regarding followups.

---
