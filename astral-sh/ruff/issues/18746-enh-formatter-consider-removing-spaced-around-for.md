```yaml
number: 18746
title: "[ENH] Formatter: consider removing spaced around `=` for `TypeVar` defaults (PEP 696)"
type: issue
state: open
author: randolf-scholz
labels:
  - formatter
  - style
assignees: []
created_at: 2025-06-18T10:29:53Z
updated_at: 2025-06-19T20:28:29Z
url: https://github.com/astral-sh/ruff/issues/18746
synced_at: 2026-01-10T11:09:58Z
```

# [ENH] Formatter: consider removing spaced around `=` for `TypeVar` defaults (PEP 696)

---

_Issue opened by @randolf-scholz on 2025-06-18 10:29_

PEP 696 introduced default values for type variables. When using in combination with PEP 695 syntax, the formatter currently always inserts spaces around the equal sign: [ruff-playground](https://play.ruff.rs/7e0f829e-94db-47f9-9490-39e1a37b65ca)

```python
def foo[
    X = Any,  # <-- spaces inserted around `=`
    Y: int = Any,  # <-- spaces inserted around `=`
](
    y=2,  # <-- spaces removed around `=`
    z: int = 2,  # <-- spaces inserted around `=`
): ...
```

My proposal is to format `TypeVar`-defaults the same way that defaults are formatted for regular function parameters, i.e. to remove the space if no `": hint"` is present[^1]. This, in my opinion, has 2 advantages:

1. It improves readability (subjective)
2. Saves 2n spaces, which can make or break whether the signature fits on a single line.

The latter is especially important when dealing with `@overload`, which are often much easier to grasp at a glance if they do not fill half a page due to spanning multiple lines per `@overload`.

Compare for instance the visuals of

```python
@overload
def combine[X = Any, Y = Any, Z = Any](
    foo: FooA[Y, Z], bar: BarA[X, Y]
) -> BazA[X, Z]: ...
@overload
def combine[X = Any, Y = Any, Z = Any](
    foo: FooB[Y, Z], bar: BarB[X, Y]
) -> BazB[X, Z]: ...
@overload
def combine[X = Any, Y = Any, Z = Any](
    foo: FooB[Y, Z], bar: BarC[X, Y]
) -> BazC[X, Z]: ...
```

and

```python
@overload
def combine[X=Any, Y=Any, Z=Any](foo: FooA[Y, Z], bar: BarA[X, Y]) -> BazA[X, Z]: ...
@overload
def combine[X=Any, Y=Any, Z=Any](foo: FooB[Y, Z], bar: BarB[X, Y]) -> BazB[X, Z]: ...
@overload
def combine[X=Any, Y=Any, Z=Any](foo: FooB[Y, Z], bar: BarC[X, Y]) -> BazC[X, Z]: ...
```

[^1]: At least when the `[...]` block only spans a single line. When it is formatted like an expanded list with 1 line per item, the spaces probably do not matter much one way or the other.






---

_Label `formatter` added by @MichaReiser on 2025-06-18 11:47_

---

_Label `style` added by @MichaReiser on 2025-06-18 11:47_

---

_Renamed from "[format] consider removing spaced around `=` for `TypeVar` defaults (PEP 696)" to "[ENH] Formatter: consider removing spaced around `=` for `TypeVar` defaults (PEP 696)" by @randolf-scholz on 2025-06-19 20:28_

---
