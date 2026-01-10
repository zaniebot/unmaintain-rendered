```yaml
number: 191
title: Combine starred element types in unpacking
type: issue
state: closed
author: dhruvmanila
labels:
  - help wanted
  - generics
assignees: []
created_at: 2025-02-25T10:34:12Z
updated_at: 2025-06-11T00:44:27Z
url: https://github.com/astral-sh/ty/issues/191
synced_at: 2026-01-10T02:34:09Z
```

# Combine starred element types in unpacking

---

_Issue opened by @dhruvmanila on 2025-02-25 10:34_

Combine starred element types. For example, in `(a, *b, c) = (1, 2, 3, 4)`, the `Literal[2]` and `Literal[3]` type should be combined into `list[int]` to be assigned to `*b`. **(Requires generic support over list)** https://github.com/astral-sh/ruff/blob/c6b311c54643c039d793fb836caf94d22f03cbb4/crates/red_knot_python_semantic/src/types/infer.rs#L1248-L1251

---

_Renamed from "Combine starred element types. For example, in `(a, *b, c) = (1, 2, 3, 4)`, the `Literal[2]` and `Literal[3]` type should be combined into `list[int]` to be assigned to `*b`. **(Requires generic support over list)** https://github.com/astral-sh/ruff/blob/c6b311c54643c039d793fb836caf94d22f03cbb4/crates/red_knot_python_semantic/src/types/infer.rs#L1248-L1251" to "[red-knot] Combine starred element type in unpacking" by @dhruvmanila on 2025-02-25 10:35_

---

_Renamed from "[red-knot] Combine starred element type in unpacking" to "[red-knot] Combine starred element types in unpacking" by @dhruvmanila on 2025-02-25 10:36_

---

_Comment by @dhruvmanila on 2025-02-25 10:52_

(Moving this to Backlog as it requires generic support over list)

---

_Renamed from "[red-knot] Combine starred element types in unpacking" to "Combine starred element types in unpacking" by @MichaReiser on 2025-05-07 15:26_

---

_Label `generics` added by @AlexWaygood on 2025-05-11 10:54_

---

_Comment by @carljm on 2025-05-30 00:36_

I think now that we have generic list, this should be pretty easy?

---

_Label `help wanted` added by @carljm on 2025-05-30 00:37_

---

_Comment by @carljm on 2025-05-30 00:39_

https://github.com/astral-sh/ty/issues/247#issuecomment-2917656676 gave this example:

```
error[unresolved-attribute]: Type `str` has no attribute `pop`
   --> src/eulertools/lib/utils.py:383:18
    |
381 |     prefix, response_key, *answers = line.split(maxsplit=2)
382 |     try:
383 |         answer = answers.pop()
    |                  ^^^^^^^^^^^
384 |     except IndexError:
385 |         answer = ""
    |
info: rule `unresolved-attribute` is enabled by default
```

It seems to be this issue, though I'm surprised we wrongly infer `str` here rather than just inferring `Unknown` or `Todo`.

---

_Comment by @dhruvmanila on 2025-05-30 03:20_

Yes, this should be easy now. I do have a couple of questions:
1. What should we infer in [this error case](https://github.com/astral-sh/ruff/blob/363f061f0920158c7eedd27825bc39f197543547/crates/ty_python_semantic/resources/mdtest/unpacking.md?plain=1#L106-L116)? Should it be `Unknown` or `list[Unknown`? I think the latter makes sense

	```py
	# error: [invalid-assignment] "Not enough values to unpack: Expected 3 or more"
	[a, *b, c, d] = (1, 2)
	reveal_type(a)  # revealed: Unknown
	# TODO: Should be list[Any] once support for assigning to starred expression is added
	reveal_type(b)  # revealed: Unknown
	reveal_type(c)  # revealed: Unknown
	reveal_type(d)  # revealed: Unknown
	```

2. Should the literal types be promoted? In [this example](https://github.com/astral-sh/ruff/blob/363f061f0920158c7eedd27825bc39f197543547/crates/ty_python_semantic/resources/mdtest/unpacking.md?plain=1#L128-L136), should the inferred type be `list[Literal[2]]` or `list[int]`?

	```py
	[a, *b, c] = (1, 2, 3)
	reveal_type(a)  # revealed: Literal[1]
	# TODO: Should be list[int] once support for assigning to starred expression is added
	reveal_type(b)  # revealed: @Todo(starred unpacking)
	reveal_type(c)  # revealed: Literal[3]
	```

3. If we do promote the literal types, it would have a side effect in [this example](https://github.com/astral-sh/ruff/blob/363f061f0920158c7eedd27825bc39f197543547/crates/ty_python_semantic/resources/mdtest/unpacking.md?plain=1#L265-L273) that we infer `list[str]` instead of `list[LiteralString]`

	```py
	(a, *b, c) = "ab"
	reveal_type(a)  # revealed: LiteralString
	# TODO: Should be list[LiteralString] once support for assigning to starred expression is added
	reveal_type(b)  # revealed: @Todo(starred unpacking)
	reveal_type(c)  # revealed: LiteralString
	```

---

_Comment by @carljm on 2025-05-30 20:29_

Re (1), I don't think it's hugely consequential either way, but I agree that if all else is equal, `list[Unknown]` is preferable.

Re (2), I would be inclined to initially not promote literals and see if it causes issues in practice. I suspect it's unusual to mutate an unpacked list, but I could be wrong.

---

_Closed by @dhruvmanila on 2025-06-03 01:55_

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:44_

---
