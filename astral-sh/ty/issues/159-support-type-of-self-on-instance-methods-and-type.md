```yaml
number: 159
title: "support type of `self` on instance methods and type of `cls` on classmethods"
type: issue
state: closed
author: carljm
labels:
  - generics
  - typing semantics
assignees: []
created_at: 2025-03-26T18:03:22Z
updated_at: 2025-12-11T18:04:39Z
url: https://github.com/astral-sh/ty/issues/159
synced_at: 2026-01-12T15:54:22Z
```

# support type of `self` on instance methods and type of `cls` on classmethods

---

_@carljm_

Proper support here overlaps with generics (since `Self` operates like a bound typevar).

- [x] Implicit annotation of `self: Self` in the signature of methods
- [x] Implicit type of `self: Self` in the body of methods
- [x] Implicit annotation of `cls: type[Self]` in the signature of `@classmethod`s
- [x] Implicit type of `cls: type[Self]` in the body of `@classmethod`s

---

_Comment by @MatthewMckee4 on 2025-04-19 11:47_

Should this need a new `Type::TypingSelf` or similarly named type?

And should `KnownInstanceType::TypingSelf` contain a type, like `ClassLiteralType`? 

---

_Comment by @carljm on 2025-04-21 14:41_

Probably worth taking a careful look at [the relevant section of the spec](https://typing.python.org/en/latest/spec/generics.html#self) if you're considering working on this.

> Should this need a new `Type::TypingSelf` or similarly named type?

I don't _think_ a new `Type` variant should be necessary here, because `Self` is specified to work the same as a typevar with an upper bound of the enclosing class type. In other words, these two are equivalent:

```py
class C1:
    def method(self: Self) -> Self: ...

class C2:
    def method[T: "C2"](self: T) -> T: ...
```

So I think that when we see an annotation of `Self`, we should be able to just construct the appropriate `Type::TypeVar` type (and add a generic context to turn the function into a generic function), without needing a new dedicated type.


> And should `KnownInstanceType::TypingSelf` contain a type, like `ClassLiteralType`?

No. `KnownInstanceType::TypingSelf` represents the object that you get at runtime when you do `from typing import Self`. It is the same everywhere that `typing.Self` is imported. It can't carry any knowledge of any particular location where `Self` is used in an annotation. Its only purpose is so that we can distinguish real usage of `typing.Self` from some random user class that happens to be named `Self`. In other words, the only special behavior `KnownInstanceType::TypingSelf` should have is its implementation of `in_type_expression`.

---

_Comment by @Glyphack on 2025-04-26 15:46_

I had a chat with Matthew and I'm working on this one.



---

_Assigned to @Glyphack by @sharkdp on 2025-04-28 06:51_

---

_Renamed from "[red-knot] support `typing.Self`, type of `self`, and type of `cls` on classmethods" to "support `typing.Self`, type of `self`, and type of `cls` on classmethods" by @MichaReiser on 2025-05-07 15:26_

---

_Added to milestone `alpha` by @MichaReiser on 2025-05-07 15:54_

---

_Closed by @carljm on 2025-05-07 22:58_

---

_Comment by @sharkdp on 2025-05-08 06:34_

Reopening this, as we now understand `typing.Self`, but not yet type of `self`

---

_Reopened by @sharkdp on 2025-05-08 06:34_

---

_Closed by @AlexWaygood on 2025-05-08 10:30_

---

_Comment by @MichaReiser on 2025-05-08 10:32_

@AlexWaygood was it intentional that you closed this issue?

---

_Comment by @AlexWaygood on 2025-05-08 10:38_

no, not at all -- all I did was sync my Ruff fork with the upstream repo

---

_Reopened by @AlexWaygood on 2025-05-08 10:38_

---

_Comment by @MichaReiser on 2025-05-08 10:39_

Hmm, interesting. I think something similar happened to me yesterday. 

---

_Comment by @AlexWaygood on 2025-05-09 10:46_

It looks like we're currently only able to solve the `Self` TypeVar if the `self` argument in a method is explicitly annotated with `Self`:

```py
from typing import Self, reveal_type

class Foo[T]:
    x: T

class Bar:
    def method1(self: Self) -> Self:
        return self

    def method2(self: Self) -> Foo[Self]:
        return Foo[Self]()

    def method3(self) -> Self:
        return self

    def method4(self) -> Foo[Self]:
        return Foo[Self]()

reveal_type(Bar().method1())  # Bar
reveal_type(Bar().method2().x)  # Bar
reveal_type(Bar().method3())  # `Unknown`, but should be `Bar`
reveal_type(Bar().method4().x)  # `Self`, but should be `Bar`
```

This is causing false positives to show up in the primer diff for https://github.com/astral-sh/ruff/pull/17832, because only a small minority of methods that include `Self` somewhere in their return annotations actually explicitly annotate the `self` argument with `Self`

---

_Comment by @carljm on 2025-05-09 14:01_

I think @Glyphack is working on inferring the type `Self` for un-annotated `self` arguments.

---

_Comment by @Glyphack on 2025-05-09 15:46_

Hey yes I'll send the pr by tomorrow latest

---

_Removed from milestone `Alpha` by @MichaReiser on 2025-05-16 12:19_

---

_Added to milestone `Beta` by @MichaReiser on 2025-05-16 12:19_

---

_Label `typing semantics` added by @dhruvmanila on 2025-05-31 09:32_

---

_Label `generics` added by @dhruvmanila on 2025-05-31 09:32_

---

_Renamed from "support `typing.Self`, type of `self`, and type of `cls` on classmethods" to "support type of `self` and type of `cls` on classmethods" by @carljm on 2025-07-25 17:25_

---

_Renamed from "support type of `self` and type of `cls` on classmethods" to "support type of `self` on instance methods and type of `cls` on classmethods" by @carljm on 2025-07-25 17:25_

---

_Comment by @hauntsaninja on 2025-08-16 22:16_

For SEO purposes, this comes up when using pandas pd DataFrame Series slicing `__getitem__`

<details>

```python
# With pandas-stubs==2.3.0.250703
from __future__ import annotations

import pandas as pd
from typing import Self, Iterable, Hashable, reveal_type, overload


def main(df: pd.DataFrame):
    reveal_type(df[df["x"] > 0])


main(pd.DataFrame({"x": [1, 2, 3]}))


def reduced1(df: pd.DataFrame, series: pd.Series[bool]):
    reveal_type(df[series])


class DF2:
    @overload
    def get(self, key: int) -> str: ...
    @overload
    def get(self, key: Iterable[Hashable]) -> Self: ...
    def get(self, *a, **kw):
        raise NotImplementedError


def reduced2(df: DF2, series: pd.Series[bool]):
    reveal_type(df.get(series))


class DF3:
    def get(self, key: int) -> Self:
        raise NotImplementedError


def reduced3(df: DF3, key: int):
    reveal_type(df.get(key))
```
</details>

---

_Comment by @Glyphack on 2025-09-02 22:14_

An update on the development. The version of ty that has implicit self (https://github.com/astral-sh/ruff/pull/18473) in method bodies has high execution time with attribute accesses in ([comment](https://github.com/astral-sh/ruff/pull/18473#issuecomment-3246087531)). I couldn't fix those and @sharkdp is figuring out a way forward.
Meanwhile I will continue https://github.com/astral-sh/ruff/pull/18007 there shouldn't be same attribute access problem for calls. So when the other one is ready we can hopefully merge both.

---

_Unassigned @Glyphack by @sharkdp on 2025-10-17 08:36_

---

_Assigned to @sharkdp by @sharkdp on 2025-10-17 08:36_

---

_Removed from milestone `Beta` by @carljm on 2025-11-12 23:37_

---

_Added to milestone `GA` by @carljm on 2025-11-12 23:37_

---

_Removed from milestone `GA` by @carljm on 2025-11-12 23:38_

---

_Added to milestone `Beta` by @carljm on 2025-11-12 23:38_

---

_Comment by @gjcarneiro on 2025-11-22 18:50_

Please be sure to also support this:

```py
from typing import Self

class Identifiable:
    @classmethod
    def with_uuid(cls, uuid: str) -> Self | None: ...

class MyModel(Identifiable):
   ...

obj = MyModel.with_uuid("xxx")
reveal_type(obj)
```

mypy returns:

```
main.py:11: note: Revealed type is "__main__.MyModel | None"
```

While Ty returns:

```
    Revealed type: `Unknown | None` (revealed-type) [Ln 11, Col 13]
```

---

_Comment by @Glyphack on 2025-11-23 12:48_

> Please be sure to also support this:

Thanks for the example. I think this will be solved with [this PR](https://github.com/astral-sh/ruff/pull/20771). Because the failing test I added there is similar to what you provided. I could not find a solution, I will spend more time on it in the next week.


---

_Assigned to @ibraheemdev by @MichaReiser on 2025-11-29 15:03_

---

_Unassigned @sharkdp by @MichaReiser on 2025-12-05 15:49_

---

_Assigned to @AlexWaygood by @MichaReiser on 2025-12-05 15:49_

---

_Comment by @carljm on 2025-12-11 18:04_

At long last, I think we can consider this one done.

---

_Closed by @carljm on 2025-12-11 18:04_

---
