```yaml
number: 14993
title: Display Union of Literals as a Literal
type: pull_request
state: merged
author: Glyphack
labels:
  - ty
assignees: []
merged: true
base: main
head: literal-union
created_at: 2024-12-15T20:31:29Z
updated_at: 2025-05-21T20:26:37Z
url: https://github.com/astral-sh/ruff/pull/14993
synced_at: 2026-01-12T15:55:49Z
```

# Display Union of Literals as a Literal

---

_@Glyphack_

## Summary

Resolves #14988

Display union of Literals like other type checkers do.

With this change we lose the sorting behavior. And we show the types as they appeared. So it's deterministic and tests should not be flaky.
This is similar to how Mypy [reveals the type](https://mypy-play.net/?mypy=latest&python=3.12&gist=51ad03b153bfca3b940d5084345e230f).

In some cases this makes it harder to know what is the order in revealed type when writing tests but since it's consistent after the test fails we know the order.

## Test Plan

I adjusted mdtests for this change. Basically merged the int and string types of the unions.

In cases where we have types other than numbers and strings like this [one](https://github.com/astral-sh/ruff/pull/14993/files#diff-ac50bce02b9f0ad4dc7d6b8e1046d60dad919ac52d0aeb253e5884f89ea42bfeL51). We only group the strings and numbers as the issue suggsted.

```
def _(flag: bool, flag2: bool):
    if flag:
        f = 1
    elif flag2:
        f = "foo"
    else:
        def f() -> int:
            return 1
    # error: "Object of type `Literal[1, "foo", f]` is not callable (due to union elements Literal[1], Literal["foo"])"
    # revealed: Unknown | int
    reveal_type(f())
```

[pyright example](https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAySMApiAIYA2AUNQCYnBQD6AFMJeWgFxQBGYMJQA0UDlwBMvAUICU3alCWYm4nouWamAXigBGDUpKUkqzmimHNYqLoBEwQXavGAziQXXlDVa1lQAWgA%2BTBQYTy9rEBIYAFcQFH0rAGIoMnAQXjsAeT4AKxIAY3wwJngEEigAAyJSCkoAbT1RBydRYABdKsxXKBQwfEKqTj5KStY6WMqYMChYlCQwROMSCBIw3tqyKiaO0S36htawOw7ZZ01U6IA3EioSOl4AVRQAa36Ad0SAH1CYKxud0ozHKJHYflk1CAA)

[mypy example](https://mypy-play.net/?mypy=latest&python=3.12&gist=31c8bdaa5521860cfeca4b92841cb3b7)

---

_Review requested from @carljm by @Glyphack on 2024-12-15 20:31_

---

_Review requested from @MichaReiser by @Glyphack on 2024-12-15 20:31_

---

_Review requested from @AlexWaygood by @Glyphack on 2024-12-15 20:31_

---

_Review requested from @sharkdp by @Glyphack on 2024-12-15 20:31_

---

_Converted to draft by @Glyphack on 2024-12-15 20:31_

---

_Label `red-knot` added by @AlexWaygood on 2024-12-15 20:53_

---

_@Glyphack reviewed on 2025-01-05 15:59_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/display.rs`:382 on 2025-01-05 15:59_

IIRC we want to move unit tests that check revealed type to mdtests so I moved it to a new test called "Display of heterogeneous unions of literals" in literals.

---

_Comment by @github-actions[bot] on 2025-01-05 16:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @Glyphack on 2025-01-05 16:10_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/display.rs`:226 on 2025-01-06 08:33_

Is this still accurate or can we clarify how including `"s"` is different from displaying `LiteralString` as `Literal[LiteralString]`?

---

_@MichaReiser approved on 2025-01-06 08:34_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/display.rs`:382 on 2025-01-06 08:36_

We don't necessarily want to move every red knot unit test to mdtest because unit tests are easier to debug, discover, and often faster, but moving this specific test makes sense. 

---

_@MichaReiser reviewed on 2025-01-06 08:36_

---

_@Glyphack reviewed on 2025-01-06 10:00_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/display.rs`:226 on 2025-01-06 10:00_

I'm not sure. I'll add a test for it this evening. 

I didn't fully understood this comment can you help me understand it please?

For example in case:
```
variable_annotation: LiteralString
```
Does this mean that we should not display `variable_annotation` as `Literal[LiteralString]`?

---

_@MichaReiser reviewed on 2025-01-06 10:04_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/display.rs`:226 on 2025-01-06 10:04_

I don't fully understand it either. That's why I commented on this PR 😆 Maybe @Slyces can help us here?

---

_@Slyces reviewed on 2025-01-07 10:56_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/display.rs`:226 on 2025-01-07 10:56_

I agree that this comment isn't very clear :/

I think my intention was following my understanding of "known literals" vs. "unknown literals" in the type enumeration:

https://github.com/astral-sh/ruff/blob/0dc00e63f444985fc55c327332602c876a2ffd60/crates/red_knot_python_semantic/src/types.rs#L543
```
/// A string literal whose value is known
StringLiteral(StringLiteralType<'db>),
/// A string known to originate only from literal values, but whose value is not known (unlike
/// `StringLiteral` above).
LiteralString,
```

My idea was probably that `Literal[l1, l2, ...]` is displaying a _set of literal values_, whereas `LiteralString` is already a set of literal values (or rather, the set of all possible literal values).
To me, displaying `Literal[LiteralString]` would be confusing and incorrect. But I can be wrong there.

After looking at the current state of the type enumeration, it seems that `LiteralString` is the only occurence of unknown literals, so if you decide to keep this comment / behaviour, it can definitely be simplified to explain the behaviour only for `LiteralString`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/display.rs`:226 on 2025-01-08 00:51_

> My idea was probably that `Literal[l1, l2, ...]` is displaying a _set of literal values_, whereas `LiteralString` is already a set of literal values (or rather, the set of all possible literal values).
> To me, displaying `Literal[LiteralString]` would be confusing and incorrect. But I can be wrong there.

I think this is exactly right. `Literal[...]` notation is for individual literal values, which need to be disambiguated from e.g. stringified annotations. That's how it's specified, and it's also what makes sense. It would be silly to display `Literal[LiteralString]` because it's redundant and unnecessarily verbose.

To me the confusing part of this comment is where it says "Not all `Literal` types are displayed using `Literal` slices", which implies that `LiteralString` is a "`Literal` type". It is a type made up of literals, but it is not a "`Literal`" type (code quotes relevant): it never uses `Literal[...]` notation at all.

I think this comment doesn't add value and it would be best to simply delete it.

```suggestion
```

---

_@carljm approved on 2025-01-08 00:53_

This looks good to me! Will delete the contentious comment and then merge :) Thank you !!

---

_Merged by @carljm on 2025-01-08 00:58_

---

_Closed by @carljm on 2025-01-08 00:58_

---

_Branch deleted on 2025-05-21 20:26_

---
