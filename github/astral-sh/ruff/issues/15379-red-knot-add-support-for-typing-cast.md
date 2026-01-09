---
number: 15379
title: "[red-knot] add support for typing.cast"
type: issue
state: closed
author: carljm
labels:
  - help wanted
  - ty
assignees: []
created_at: 2025-01-09T17:55:14Z
updated_at: 2025-01-15T13:04:54Z
url: https://github.com/astral-sh/ruff/issues/15379
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] add support for typing.cast

---

_Issue opened by @carljm on 2025-01-09 17:55_

This should be pretty simple to add, once support for `assert_type` (and functions that take some value expressions and some type expressions) lands.

---

_Label `help wanted` added by @carljm on 2025-01-09 17:55_

---

_Label `red-knot` added by @carljm on 2025-01-09 17:55_

---

_Comment by @InSyncWithFoo on 2025-01-10 18:54_

I'm working on this. It turned out not to be so simple, due to how `bind_call()` handles `cast()`'s signature:

```python
cast(str, True)
```

```rust
let Some((casted_ty, _)) = binding.two_parameter_tys() else { /* ... */ };
//        ^^^^^^^^^ str | Literal[True]
```

A possible reason is that [`cast()` is defined using overloads](https://github.com/python/typeshed/blob/f26ad2059ef33e7aa55a15bf861f594e441de03b/stdlib/typing.pyi#L892-L897) and `bind_call()` can't handle overloads yet:

```py
@overload
def cast(typ: type[_T], val: Any) -> _T: ...
@overload
def cast(typ: str, val: Any) -> Any: ...
@overload
def cast(typ: object, val: Any) -> Any: ...
```

---

_Comment by @InSyncWithFoo on 2025-01-10 19:01_

Open question: Should there be a "strict casting" mode in which a value cannot be casted to a type with which its own inferred type does not overlap (or of which it is not a supertype/subtype)?

Basedpyright calls this [`reportInvalidCast`](https://docs.basedpyright.com/latest/benefits-over-pyright/new-diagnostic-rules/#reportinvalidcast).

---

_Comment by @carljm on 2025-01-10 20:22_

> turned out not to be so simple, due to how bind_call() handles cast()'s signature

Oh! Yeah, this is because we don't understand overloads, so we treat it as a function defined with an unknown decorator, meaning we currently replace its signature with the "todo signature" `(*args: Todo, **kwargs: Todo) -> Todo`. So then both positional arguments are bound to the `*args` parameter. 

Short of supporting overloads, which will be a somewhat bigger feature (though we should do it soon), I think the easiest workaround here is to change our definition of the todo signature to `(first: Todo = Todo, *args: Todo, **kwargs: Todo) -> Todo`. That is, add a first positional argument with default. This signature still accepts any call, but it distinguishes the first parameter such that we can pull out its type separately from call binding. 

---

_Comment by @carljm on 2025-01-10 20:24_

> Should there be a "strict casting" mode

I wouldn't add that now. We can certainly consider it as a feature later, but it's not a "table stakes" feature that we need in order to be usable. It's an opinionated extension of the way `cast` is specified and works in mypy and pyright. 

---

_Referenced in [astral-sh/ruff#15413](../../astral-sh/ruff/pulls/15413.md) on 2025-01-11 05:19_

---

_Closed by @AlexWaygood on 2025-01-12 13:05_

---

_Comment by @KotlinIsland on 2025-01-15 13:04_

basedmypy also reports errors with non-overlapping casts
https://mypy-play.net/?mypy=basedmypy-latest&python=3.12&gist=4642a46d5933b8aa9628918383597bc3

---
