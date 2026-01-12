```yaml
number: 15490
title: "[red-knot] More comprehensive 'is_subtype_of' tests"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/is_subtype_of-tests
created_at: 2025-01-15T09:47:52Z
updated_at: 2025-01-15T18:34:59Z
url: https://github.com/astral-sh/ruff/pull/15490
synced_at: 2026-01-12T15:55:51Z
```

# [red-knot] More comprehensive 'is_subtype_of' tests

---

_@sharkdp_

## Summary

Trying to make the `is_subtype_of` tests a bit easier to understand and more comprehensive.


---

_Label `testing` added by @sharkdp on 2025-01-15 09:47_

---

_Label `red-knot` added by @sharkdp on 2025-01-15 09:47_

---

_Review requested from @carljm by @sharkdp on 2025-01-15 09:47_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-15 09:47_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-15 09:47_

---

_Comment by @github-actions[bot] on 2025-01-15 09:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review request for @MichaReiser removed by @MichaReiser on 2025-01-15 10:01_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:21 on 2025-01-15 14:57_

This one surprises me.  I know that `bool` can e.g. participate in arithmetic with integers, but I thought that `True`/`False` were distinct values from `1`/`0`, and so these types would be disjoint.  Is this because internally we treat `int` as sugar for `int|bool` and `float` as sugar for `float|int|bool`?

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:28 on 2025-01-15 15:03_

This might answer my question above.  Is this meant to be a link?  There is no link footnote giving it a URL.  And since this is in a code block it won't be rendered as a link even if there were.  Maybe replace this with (or add) a URL?

---

_@dcreager reviewed on 2025-01-15 15:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:21 on 2025-01-15 15:07_

`bool` is a subtype of `int` in the type system because `bool` is a subclass of `bool` at runtime! All instances of `bool` (there are exactly 2) are also instances of `int` at runtime:

```py
>>> bool.__mro__
(<class 'bool'>, <class 'int'>, <class 'object'>)
>>> isinstance(False, int)
True
>>> isinstance(True, int)
True
```

This means that all methods and attributes available on `int`s are also available on `bool`s

---

_@AlexWaygood reviewed on 2025-01-15 15:07_

---

_@dcreager reviewed on 2025-01-15 15:10_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:21 on 2025-01-15 15:10_

:exploding_head: 

---

_@AlexWaygood reviewed on 2025-01-15 15:13_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:21 on 2025-01-15 15:13_

This is seen by many (including me) as a wart in the language; I think if Python were written from scratch in 2025, it's likely that `bool` would not be a subclass of `int`. It's impossible to reverse now due to backwards-compatibility implications, though.

It results in lots of "fun" behaviour at runtime:

```pycon
>>> 1 == True
True
>>> d = {1: "foo"}
>>> d[True] = "bar"
>>> d
{1: 'bar'}
>>> ~True
-2
```

---

_@sharkdp reviewed on 2025-01-15 15:24_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:28 on 2025-01-15 15:24_

Oh. This was supposed to link to https://typing.readthedocs.io/en/latest/spec/special-types.html#special-cases-for-float-and-complex, but since it only appears in a code block, mdformat removed my link at the bottom of the document :angry: 

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:28 on 2025-01-15 15:33_

Fixed, thanks.

---

_@sharkdp reviewed on 2025-01-15 15:33_

---

_@AlexWaygood reviewed on 2025-01-15 15:33_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:95 on 2025-01-15 15:33_

this doesn't link either unfortunately as it's a code block 😢 maybe just paste in the raw URL here?

---

_@carljm reviewed on 2025-01-15 17:33_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:21 on 2025-01-15 17:33_

Even at the time, it wasn't designed this way because anyone thought it was the Right design in the abstract. It was done this way because Python didn't originally have a boolean type at all, and it was common practice to use `1` and `0` for boolean values, so when a boolean type was introduced it was felt that compatibility with this practice was important.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:5 on 2025-01-15 17:34_

```suggestion
A fully static type `S` is a subtype of another fully static type `T` iff the set of values
```

---

_@carljm approved on 2025-01-15 18:17_

😍 

---

_Merged by @sharkdp on 2025-01-15 18:33_

---

_Closed by @sharkdp on 2025-01-15 18:33_

---

_Branch deleted on 2025-01-15 18:33_

---
