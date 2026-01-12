```yaml
number: 1198
title: literal promotion in inference fails to account for type context
type: issue
state: closed
author: carljm
labels:
  - generics
  - bidirectional inference
assignees: []
created_at: 2025-09-18T00:13:22Z
updated_at: 2025-10-09T20:53:55Z
url: https://github.com/astral-sh/ty/issues/1198
synced_at: 2026-01-12T15:54:24Z
```

# literal promotion in inference fails to account for type context

---

_@carljm_

> A similar issue came up in https://github.com/astral-sh/ruff/pull/20360. We now unconditionally promote any literals within the elements of a list/set literal, partly for performance reasons with large list literals. However, this means that we fail to typecheck against literal annotations, e.g.,
> ```py
> # error: Object of type `list[int]` is not assignable to `list[Literal[1, 2, 3]]`
> x: list[Literal[1, 2, 3]] = [1, 2, 3]
> 
> # error: Object of type `list[str]` is not assignable to `list[LiteralString]`
> x: list[LiteralString] = ["a", "b", "c"]
> ```
> 
> Which is the same issue we currently run into with generic classes:
> ```py
> # error: Object of type `tuple[X[int]]` is not assignable to `tuple[X[Literal[1]]]`
> x: tuple[X[Literal[1]]] = (X(1), )
> ```
> 
> My reading of this issue is that we promote *should* eagerly promote literals, unless there is an explicit type annotation that would require otherwise? That seems to align with pyright's behavior as well.
> 
> @carljm pointed out that this could get tricky with nested types, or even unions:
> ```py
> x: list[int | Literal["a"]] = [1, 2, 3, "a", 4, 5, 6]
> reveal_type(x)
> ```

 _Originally posted by @ibraheemdev in [#336](https://github.com/astral-sh/ty/issues/336#issuecomment-3304801943)_

---

_Added to milestone `GA` by @carljm on 2025-09-18 00:13_

---

_Label `generics` added by @carljm on 2025-09-18 00:13_

---

_Label `bidirectional inference` added by @carljm on 2025-09-18 00:13_

---

_Assigned to @ibraheemdev by @ibraheemdev on 2025-09-18 00:26_

---

_Removed from milestone `GA` by @ibraheemdev on 2025-09-19 14:35_

---

_Added to milestone `Beta` by @ibraheemdev on 2025-09-19 14:35_

---

_Closed by @ibraheemdev on 2025-10-09 20:53_

---
