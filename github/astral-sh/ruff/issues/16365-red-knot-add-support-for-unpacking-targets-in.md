---
number: 16365
title: "[red-knot] Add support for unpacking targets in `with` statement"
type: issue
state: closed
author: dhruvmanila
labels:
  - help wanted
  - ty
assignees: []
created_at: 2025-02-25T10:33:50Z
updated_at: 2025-03-08T02:36:37Z
url: https://github.com/astral-sh/ruff/issues/16365
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] Add support for unpacking targets in `with` statement

---

_Issue opened by @dhruvmanila on 2025-02-25 10:33_

### Description

For example:

```py
class Unpacked:
    def __enter__(self) -> tuple[int, int]:
        return (1, 2)

    def __exit__(self, exc_type, exc_value, exc_tb):
        return


with Unpacked() as (x, y):
    reveal_type(x)  # revealed: int
    reveal_type(y)  # revealed: int
```

---

_Label `red-knot` added by @dhruvmanila on 2025-02-25 10:33_

---

_Comment by @MichaReiser on 2025-02-25 11:21_

Is this something a contributor could work on or is there some hidden complexity?

---

_Comment by @dhruvmanila on 2025-02-25 11:32_

I plan to spend a couple of minutes today to check if it's "help wanted" or not. I'll revert back here once that's done.

---

_Comment by @dhruvmanila on 2025-02-26 09:33_

I took a look at the inference side because the semantic side is straightforward. My main concern is about how to infer the context expression inside the `Unpacker`. See below after the linked code.

The implementation would be similar to https://github.com/astral-sh/ruff/pull/15058. What needs to happen is:

- Update `CurrentAssignment::WithItem` with a `unpack` field similar to `For`
- Update `WithItemDefinitionNodeRef` with a `unpack` field similar to `ForStmtDefinitionNodeRef`
- Update `WithItemDefinitionKind` with a `unpack` field similar to `ForStmtDefinitionKind`
- Update this field in `visit_stmt` inside `SemanticIndexBuilder` if the `optional_vars` is a tuple
- Update `infer_with_item_definition` to infer the unpacked assignment target similar to `infer_for_statement_definition`

https://github.com/astral-sh/ruff/blob/bf2c9a41cd7e5860a6aea8b9789893767ff4c1a5/crates/red_knot_python_semantic/src/types/infer.rs#L1580-L1600

Now, we need to infer the context expression inside `Unpacker::unpack`. The reason for this is that `infer_unpack_types` is a salsa query for which the ingredient is `Unpack` but that doesn't contain the value type that needs to be unpacked, so the inference of that is done inside the `Unpacker`.

For `with` statement, I think we'd need a new `UnpackValue` variant, extract `TypeInferenceBuilder::infer_context_expression` as a standalone function which is then called in (a) inside `Unpacker::unpack` when the definition target is `Sequence` and (b) inside `infer_with_item_definition` when the definition target is `Name`.

Writing this down, it does seems straightforward.

---

_Label `help wanted` added by @dhruvmanila on 2025-02-26 09:36_

---

_Comment by @ericmarkmartin on 2025-03-01 04:14_

I'd like to give this a try

---

_Assigned to @ericmarkmartin by @AlexWaygood on 2025-03-01 18:15_

---

_Referenced in [astral-sh/ruff#16469](../../astral-sh/ruff/pulls/16469.md) on 2025-03-03 06:28_

---

_Closed by @carljm on 2025-03-08 02:36_

---

_Closed by @carljm on 2025-03-08 02:36_

---
