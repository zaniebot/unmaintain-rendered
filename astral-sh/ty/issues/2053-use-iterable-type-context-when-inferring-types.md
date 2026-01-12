```yaml
number: 2053
title: Use Iterable type context when inferring types inside a literal tuple
type: issue
state: open
author: gpajot
labels:
  - generics
  - bidirectional inference
assignees: []
created_at: 2025-12-18T07:56:54Z
updated_at: 2026-01-08T19:12:46Z
url: https://github.com/astral-sh/ty/issues/2053
synced_at: 2026-01-12T15:54:26Z
```

# Use Iterable type context when inferring types inside a literal tuple

---

_@gpajot_

### Summary

This has some similarities with https://github.com/astral-sh/ty/issues/1576 at first glance.

[Playground link](https://play.ty.dev/19eadbf5-16e1-49e6-bc2b-f9f7743df92e)

It fails in all those circumstances:

- `float` is replaced by any union type (`float` itself is transformed to `int | float` as per the error message)
- `Iterable` is replaced by `Sequence` (and maybe other `collections.abc` types?)
- input value is replaced by `set` or `list`
- `dataclass` is replaced by either a normal generic class or a generic pydantic model

It does work if we replace `Iterable` by a concrete container class such as `tuple`.
It also works if replacing the generic data class by a generic built-in type such as `list[T]`.

Thanks for the tremendous work by the way!



### Version

ty 0.0.3 (06305f3c0 2025-12-18)

---

_Label `generics` added by @sharkdp on 2025-12-18 09:41_

---

_Comment by @sharkdp on 2025-12-18 09:45_

Thank you for reporting this.

This seems like a problem that is related to invariance and the [float special case](https://docs.astral.sh/ty/reference/typing-faq/#why-does-ty-show-int-float-when-i-annotate-something-as-float).

The error goes away if I change `v: T` to `_v: T` to make `Value` covariant in `T`.

Maybe it's also related to bidirectional type-checking (involving generic protocols), because this works just fine:
```py
def accepts_single_value(value: Value[float]): ...

accepts_single_value(Value(1.))
```

---

_Comment by @gpajot on 2025-12-18 10:56_

Right 👍 thanks for the replay, makes sense. Using `Final[T]` works as well, though specifying the data class as frozen doesn't seem to make it infer the proper variance.

---

_Comment by @carljm on 2025-12-22 23:26_

Several things going on here!

First of all, `frozen=True` doesn't make the dataclass covariant due to the `__replace__` method added in Python 3.13: all dataclasses (even frozen ones) end up inferred as invariant. Pyright has the same behavior. This is unfortunate and a known open issue in Python dataclass typing. Under Python 3.12 we [infer it as covariant](https://play.ty.dev/d95f51aa-fc93-4062-b046-63f5c8954d19).

Given invariance, it is correct that `tuple[Value[float], Value[float]]` is not assignable to `Iterable[Value[float | int]]`. The discrepancy between `float` in the first case and `float | int` in the second case is because of the special case where `float` in a type expression is interpreted as `float | int`, but we infer `Value[float]` (not `Value[float | int]`) from the literal float in `Value(0.1)`. We could consider having less precise inference of float literals in general, to help with invariance cases like this, but this seems unfortunate.

Alternatively, we could apply type context so that we choose a better inference for these particular instantiations of `Value`.

It may help clarify if we transform the example to this version, using `int` and `bool` instead, so as to avoid the `float | int` special case.

```py
from collections.abc import Iterable
from dataclasses import dataclass

@dataclass(frozen=True)
class Value[T]:
    v: T


def func(values: Iterable[Value[int]]) -> None:
    for v in values:
        print(v)

func((Value(True), Value(False)))
```

This passes in pyright, due to bidirectional inference giving a hint that `Value[True]` and `Value[False]` should be inferred as `Value[int]` not `Value[bool]`. (We can see that bidirectional inference is the key by [extracting the argument expression to a separate line](https://pyright-play.net/?pythonVersion=3.14&strict=true&code=GYJw9gtgBAxmA28CmMAuBLMA7AzgOgEMAjGKdCABzBFSgElUkRjkAoUSKAEwNQJngEcOJDjKVqtHnwFCcrVgAFp-QcIAUHAF5IsAXgAqIAK5IAlK1nCoANQLxTAbQMBdAFysoXqADc3UAwVWLiRgKGBjLBh1H3tTHH8GJhYkRzsHVPQsVBcXMygAWgA%2BKAA5bCQPb3DqXzIsXzjRKurvChAs1BiLVgAPKD0odXTTdSNTMwAaWyb1ADF7ETMeiKj1XrMgA), causing the type context to be lost, and making this an error in Pyright.)

The current PR for #1576 doesn't quite fix this for us. It fixes it (even with floats) if we use a list instead of a tuple at the call site:

```py
from collections.abc import Iterable
from dataclasses import dataclass

@dataclass(frozen=True)
class Value[T]:
    v: T


def func(values: Iterable[Value[float]]) -> None:
    for v in values:
        print(v)

func([Value(0.1), Value(0.2)])
```

But it looks like we'll need some additional work to apply the `Iterable[Value[float]]` type context to the `Value` instantiations inside a tuple.

Keeping this issue open (separate from #1576) to track that piece.


---

_Renamed from "`Iterable[<GenericClass>[<Union>]]` not accepting concrete implementations" to "Use Iterable type context when inferring types inside a literal tuple" by @carljm on 2025-12-22 23:27_

---

_Label `bidirectional inference` added by @carljm on 2025-12-22 23:27_

---

_Added to milestone `Stable` by @carljm on 2025-12-22 23:27_

---

_Comment by @carljm on 2026-01-08 19:12_

The original OP example here works now, because we now widen `float` to `float | int` wherever we would widen literals. But this example still doesn't work, because we aren't actually pushing down the type context into the tuple:

```py
from collections.abc import Iterable
from dataclasses import dataclass

@dataclass(frozen=True)
class Value[T]:
    v: T


def func(values: Iterable[Value[int]]) -> None:
    for v in values:
        print(v)

func((Value(True), Value(False)))
```

---
