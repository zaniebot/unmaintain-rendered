```yaml
number: 16570
title: "`FURB140 `: `reimplemented-starmap` and static type analysis"
type: issue
state: open
author: tpgillam
labels:
  - question
  - rule
  - preview
assignees: []
created_at: 2025-03-08T20:45:17Z
updated_at: 2025-03-14T14:51:15Z
url: https://github.com/astral-sh/ruff/issues/16570
synced_at: 2026-01-12T15:54:55Z
```

# `FURB140 `: `reimplemented-starmap` and static type analysis

---

_@tpgillam_

A small question/discussion point (relevant as-of ruff 0.9.10 preview). Pertains to how `reimplemented-starmap` [FURB140](https://docs.astral.sh/ruff/rules/reimplemented-starmap/) recommends code that interacts less well with certain static type checkers.

Consider the following example:

```python
import itertools


def moo(x: int, y: str) -> str:
    return f"{x} {y}"


x = [(1, "a"), (2, "b")]
y = [(1, "a", False), (2, "b", False)]


moo_x1 = list(itertools.starmap(moo, x))
moo_y1 = list(itertools.starmap(moo, y))  # (no pyright error)

moo_x2 = [moo(a, b) for a, b in x]
moo_y2 = [moo(a, b) for a, b in y]  # ERROR: Expression with type "tuple[int, str, bool]" cannot be assigned...
```
- The creation of `moo_y1` and `moo_y2` both fail (expectedly) at runtime.
- pyright 1.1.396 spots the problem with the comprehension. But with starmap pyright can't analyse it.

`reimplemented-starmap` will want to change the second forms (using the comprehensions) to the first (with starmap). We currently use pyright for static analysis, so from _our_ perspective this suggested change arguably results in more fragile, less verified, code.

- Should we say that FURB140 is good, and that the static type checker needs to be improved? And that it could/should in theory catch this?
- Is this something that should be discussed upstream in `refurb`? e.g. since ruff's job is just to mirror the rule that's implemented there?

I've not previously been inclined to disable a ruff rule because I felt that applying it could result in more fragile code. Hence I thought I'd open this up for discussion.

---

_Label `question` added by @tpgillam on 2025-03-08 20:45_

---

_Label `rule` added by @MichaReiser on 2025-03-14 08:20_

---

_Label `preview` added by @MichaReiser on 2025-03-14 08:20_

---

_Renamed from "`reimplemented-starmap` and static type analysis" to "`FURB140 `: `reimplemented-starmap` and static type analysis" by @MichaReiser on 2025-03-14 08:20_

---

_Comment by @MichaReiser on 2025-03-14 08:21_

Thanks for opening this issue. This is very important feedback and getting it before we stabilize rules is very valuable! 

@carljm sorry for pulling you in. Do you know if this is a common limitation or more likely to be specific to pyright?

---

_Comment by @tpgillam on 2025-03-14 09:38_

Having a peek at the [typeshed annotations for starmap](https://github.com/python/typeshed/blob/5c85697da7c24bb70d72e9883004f2974a3b7e46/stdlib/itertools.pyi#L105C7-L108) suggests that the argument types for the callable won't be constrained. (Not sure if other tools might provide more constrained annotations for the stdlib though)

---

_Comment by @carljm on 2025-03-14 13:58_

I doubt that any tools provide special-cased handling of `starmap`, so I suspect that whatever the typeshed annotations specify is how tools will behave.

It looks like the typeshed annotation of starmap could be improved using `TypeVarTuple` sufficiently to catch this case? Not entirely sure without trying it out.

---

_Comment by @tpgillam on 2025-03-14 14:51_

Toying around with this a little:

```python
class StarMap[T]:
    @overload
    def __init__[*Ts](
        self, function: Callable[[*Ts], T], iterable: Iterable[tuple[*Ts]]
    ) -> None: ...
    @overload
    def __init__(
        self, function: Callable[..., T], iterable: Iterable[Iterable[Any]]
    ) -> None: ...
    def __init__(
        self, function: Callable[..., T], iterable: Iterable[Iterable[Any]]
    ) -> None:
        self._starmap = itertools.starmap(function, iterable)

    def __iter__(self) -> Self:
        self._starmap.__iter__()
        return self

    def __next__(self) -> T:
        return self._starmap.__next__()
```

- the first overload is a more specific one using a `TypeVarTuple`
- we have to keep the `Iterable[Iterable[Any]]` case too, so as to support e.g. `starmap(moo, [[1, "a"], [2, "b"]]` where the type system doesn't know about the elements

The 'knowably bad' case won't match the new overload, but will still match the second. I'm not sure if there's a way to have the extra constraint without disallowing other statically-unknown but valid uses!

---
