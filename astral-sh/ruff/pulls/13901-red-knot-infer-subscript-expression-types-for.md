```yaml
number: 13901
title: "[red-knot] Infer subscript expression types for bytes literals"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/infer-subscript-expr-for-bytes
created_at: 2024-10-24T07:38:03Z
updated_at: 2024-10-24T10:07:52Z
url: https://github.com/astral-sh/ruff/pull/13901
synced_at: 2026-01-10T20:59:37Z
```

# [red-knot] Infer subscript expression types for bytes literals

---

_Pull request opened by @sharkdp on 2024-10-24 07:38_

## Summary

Infer subscript expression types for bytes literals:
```py
b = b"\x00abc\xff"

reveal_type(b[0])  # revealed: Literal[b"\x00"]
reveal_type(b[1])  # revealed: Literal[b"a"]
reveal_type(b[-1])  # revealed: Literal[b"\xff"]
reveal_type(b[-2])  # revealed: Literal[b"c"]

reveal_type(b[False])  # revealed: Literal[b"\x00"]
reveal_type(b[True])  # revealed: Literal[b"a"]
```


part of #13689 (https://github.com/astral-sh/ruff/issues/13689#issuecomment-2404285064)

## Test Plan

- New Markdown-based tests (see `mdtest/subscript/bytes.md`)
- Added missing test for `string_literal[bool_literal]`

---

_Label `red-knot` added by @sharkdp on 2024-10-24 07:38_

---

_Review requested from @carljm by @sharkdp on 2024-10-24 07:38_

---

_Review requested from @MichaReiser by @sharkdp on 2024-10-24 07:38_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-10-24 07:38_

---

_@sharkdp reviewed on 2024-10-24 07:38_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/subscript/bytes.md`:9 on 2024-10-24 07:38_

These were placed in the wrong file => moved to `mdtest/literal/bytes.md`.

---

_@sharkdp reviewed on 2024-10-24 07:41_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/subscript/string.md`:24 on 2024-10-24 07:41_

I am assuming `add` was intended to be a function that definitely returns an `int`, not a `LiteralInt`. Elsewhere, we just use `def int_instance() -> int: ...` for this. Let me know if I'm misunderstanding the intention.

---

_@sharkdp reviewed on 2024-10-24 07:41_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/subscript/string.md`:30 on 2024-10-24 07:41_

I understand that we want to infer `str` here. But I don't understand the "Support overloads" comment. Can someone explain?

---

_Comment by @github-actions[bot] on 2024-10-24 07:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@sharkdp reviewed on 2024-10-24 08:02_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3176 on 2024-10-24 08:02_

For slice expressions, I intend to move both `iterator_at_index` and `slice_at_index` to a separate module, which will then also include functionality to do something similar for slices. I then also intend to add some unit tests for these functions, so maybe don't pay too much attention to these functions for now.

---

_@AlexWaygood reviewed on 2024-10-24 08:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/subscript/string.md`:30 on 2024-10-24 08:23_

We attempt to infer the type here by looking up [`str.__getitem__` in typeshed](https://github.com/python/typeshed/blob/c417573eb6f59c8f40b7c0dc7b1d75549b8f6e59/stdlib/builtins.pyi#L592) and looking at what that method is annotated as returning. But `str.__getitem__` is an [overloaded function](https://docs.python.org/3/library/typing.html#typing.overload) in typeshed, and we don't yet have the ability to understand those.

In fact, we just infer `@Todo` as the return type for all decorated functions currently, since a decorated function (and its return type) is transformed by its decorator(s) before the user sees it. That means it's less simple than just "believe the return type annotation" when it comes to understanding the return type of decorated functions.

---

_@sharkdp reviewed on 2024-10-24 08:44_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3176 on 2024-10-24 08:44_

I decided to add that to this branch after all.

---

_@sharkdp reviewed on 2024-10-24 08:47_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/subscript/string.md`:30 on 2024-10-24 08:47_

Thanks!

---

_@sharkdp reviewed on 2024-10-24 09:37_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3176 on 2024-10-24 09:37_

And merged the two functions into a single one.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/util/subscript.rs`:1 on 2024-10-24 09:45_

I probably would have just had this module as `red_knot_python_semantic/src/types/subscript.rs`, but don't feel strongly

---

_@AlexWaygood approved on 2024-10-24 09:47_

Nice!! I love the trait solution

---

_@sharkdp reviewed on 2024-10-24 10:07_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/util/subscript.rs`:1 on 2024-10-24 10:07_

indexing/slicing to me feels like something that is not really related to types, but rather to "const evaluation" of some expressions at type-check time. I'll leave it where it is for now, we can decide again in the next PR which is going to add slices to this module.

---

_Merged by @sharkdp on 2024-10-24 10:07_

---

_Closed by @sharkdp on 2024-10-24 10:07_

---

_Branch deleted on 2024-10-24 10:07_

---

_@AlexWaygood reviewed on 2024-10-24 10:07_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/util/subscript.rs`:1 on 2024-10-24 10:07_

SGTM

---
