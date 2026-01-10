---
number: 2251
title: Emit error when unpacking a not-iterable argument in a call
type: issue
state: open
author: phistep
labels:
  - bug
assignees: []
created_at: 2025-12-28T23:25:13Z
updated_at: 2026-01-06T02:15:14Z
url: https://github.com/astral-sh/ty/issues/2251
synced_at: 2026-01-10T01:51:14Z
---

# Emit error when unpacking a not-iterable argument in a call

---

_Issue opened by @phistep on 2025-12-28 23:25_

### Summary

```py
from typing import Generic, TypeAlias, TypeVar


def foo(a: str, b: int, c: int, d: str): ...


VType: TypeAlias = int | tuple[int, int]
V = TypeVar("V", bound=VType)


class Foo(Generic[V]):
    v: V

    def __init__(self, v: V):
        self.v = v


D = dict(
    a=Foo(v=1),
    b=Foo(v=(2, 2)),
)
foo("foo", *D["b"], d="bar")
```

reports
```
error[parameter-already-assigned]: Multiple values provided for parameter `d` of function `foo`
  --> src/vhsh/test.py:22:21
   |
20 |     b=Foo(v=(2, 2)),
21 | )
22 | foo("foo", *D["b"], d="bar")
   |                     ^^^^^^^
   |
info: rule `parameter-already-assigned` is enabled by default

Found 1 diagnostic
```
when actually `d` is only passed once and `D["b"]` is unpacked into `b` _and_ `c`.

This is similar to #2250 and #1985 but produces an error message and not only misleading inlays.

<img width="699" height="532" alt="Image" src="https://github.com/user-attachments/assets/dceaa659-3762-466d-94b4-f56b910b9877" />


---

```py
foo("foo", D["b"][0], D["b"][1], d="bar")
```
gives the expected[^1] errors

```
error[non-subscriptable]: Cannot subscript object of type `Foo[int]` with no `__getitem__` method
  --> src/vhsh/test.py:22:12
   |
20 |     b=Foo(v=(2, 2)),
21 | )
22 | foo("foo", D["b"][0], D["b"][1], d="bar")
   |            ^^^^^^^^^
   |
info: rule `non-subscriptable` is enabled by default

error[non-subscriptable]: Cannot subscript object of type `Foo[tuple[int, int]]` with no `__getitem__` method
  --> src/vhsh/test.py:22:12
   |
20 |     b=Foo(v=(2, 2)),
21 | )
22 | foo("foo", D["b"][0], D["b"][1], d="bar")
   |            ^^^^^^^^^
   |
info: rule `non-subscriptable` is enabled by default

error[non-subscriptable]: Cannot subscript object of type `Foo[int]` with no `__getitem__` method
  --> src/vhsh/test.py:22:23
   |
20 |     b=Foo(v=(2, 2)),
21 | )
22 | foo("foo", D["b"][0], D["b"][1], d="bar")
   |                       ^^^^^^^^^
   |
info: rule `non-subscriptable` is enabled by default

error[non-subscriptable]: Cannot subscript object of type `Foo[tuple[int, int]]` with no `__getitem__` method
  --> src/vhsh/test.py:22:23
   |
20 |     b=Foo(v=(2, 2)),
21 | )
22 | foo("foo", D["b"][0], D["b"][1], d="bar")
   |                       ^^^^^^^^^
   |
info: rule `non-subscriptable` is enabled by default

Found 4 diagnostics
```

<img width="627" height="134" alt="Image" src="https://github.com/user-attachments/assets/83cd6b65-aeb0-4ea0-87d1-22e36a6ec14d" />

[^1]: sadly unsatisfactory due to impossible type inference w/o `TypedDict`.


### Version

`ty 0.0.7 (cf82a04b5 2025-12-24)`

---

_Comment by @MichaReiser on 2025-12-29 11:15_

@dhruvmanila do you know what's happening here?

---

_Comment by @dhruvmanila on 2025-12-29 12:05_

I think this might be related to https://github.com/astral-sh/ty/issues/1584, what's happening is that `D["b"]` cannot be unpacked because it's not an iterable (the type is `Foo[int] | Foo[tuple[int, int]]` as seen in the inlay hint of `D`), so it must be unpacking into `Unknown`s, but we don't know how many elements are there in the unpacking so it has matched all of the remaining arguments by the time we reach to `d="bar"` which is why you're seeing `parameter-already-assigned` error.

> `D["b"]` is unpacked into `b` _and_ `c`.

I don't think this is correct, refer to my previous paragraph. For a literal dictionary creation, ty will infer the general type of `dict[str, <union of value types>]`.

---

_Comment by @carljm on 2025-12-29 16:34_

Yeah, the implied request here is to do some kind of implicit `TypedDict` inference of dict literals -- which we could do, but it's quite tricky to handle subsequent mutation without either introducing false positives or false negatives. This is tracked in https://github.com/astral-sh/ty/issues/1248

Barring that, it does seem like there are two bugs in ty here. One is discussed above (and tracked in #1584): given we don't know which arguments `D["b"]` will unpack to, we should prefer to assume a valid call, not assume it eats all arguments, including those otherwise explicitly provided.

The second is, we should emit an error on the attempt to unpack `D["b"]` (as [pyright does](https://pyright-play.net/?pyrightVersion=1.1.405&pythonVersion=3.13&reportUnreachable=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoDiApikSEgMYA0UAKokQIIA2SAhgM430JEBqbEACgRAEyLAowMGAAUbAFxQOMEDQBGS1DBoUtKHVFFKVIAJRKAdNZFC%2BPIkoct2HKAF5MBqAB9YAVwRmIgBtbRptAF07DzoGARBZACI%2BJI0wfxRRd3sGM1sKZk43ADEZWWJScgoQvkiLISgmqAA3JT4RZqMJKAB9XtQkGH7ZDiJmYBo2qD4Grq6xicsW2JbbABFY0UoYWUbmtncyuRb3AEYzKn2m9SPy09kAJhpHs0uhfOk5JK%2B0qAAqdYhJLqJKRGjZEGCJJmIA)) -- that would help clarify what's going on. We can use this issue to track that.





---

_Renamed from "Misleading error message when using argument unpacking of Unknown" to "Emit error when unpacking a not-iterable argument in a call" by @carljm on 2025-12-29 16:35_

---

_Added to milestone `Stable` by @carljm on 2025-12-29 16:35_

---

_Label `bug` added by @dhruvmanila on 2025-12-30 05:38_

---

_Comment by @phistep on 2025-12-30 23:06_

Thank you for replying so quicky!

@dhruvmanila 
> > `D["b"]` is unpacked into `b` _and_ `c`.
>
> I don't think this is correct, refer to my previous paragraph. For a literal dictionary creation, ty will infer the general type of `dict[str, <union of value types>]`.

I'm sorry, I made a typo when boiling down my code to the MWE. I meant to pass `.v` acutally, so that the unpacking would make sense

> `D["b"].v` (_sic!_) is unpacked into `b` _and_ `c`.

```python
foo("foo", *D["b"].v, d="bar")
```

<img width="496" height="82" alt="Image" src="https://github.com/user-attachments/assets/dff3708a-16a6-4535-92a8-96a26de019a0" />

Then the report is

```
error[invalid-argument-type]: Argument to function `foo` is incorrect
  --> test.py:22:12
   |
20 |     b=Foo(v=(2, 2)),
21 | )
22 | foo("foo", *D["b"].v, d="bar")
   |            ^^^^^^^^^ Expected `str`, found `int`
   |
info: Function defined here
 --> test.py:4:5
  |
4 | def foo(a: str, b: int, c: int, d: str): ...
  |     ^^^                         ------ Parameter declared here
  |
info: rule `invalid-argument-type` is enabled by default

error[parameter-already-assigned]: Multiple values provided for parameter `d` of function `foo`
  --> test.py:22:23
   |
20 |     b=Foo(v=(2, 2)),
21 | )
22 | foo("foo", *D["b"].v, d="bar")
   |                       ^^^^^^^
   |
info: rule `parameter-already-assigned` is enabled by default

Found 2 diagnostics
```

Which is wrong and misleading in a different way...

---

Whereas this givers the correct error message (where it is sad but understanable that no implicit TypedDict style inference can take place)

```py
foo("foo", D["b"].v[0], D["b"].v[1], d="bar")
```

```
error[not-subscriptable]: Cannot subscript object of type `int` with no `__getitem__` method
  --> test.py:24:12
   |
22 | v = D["b"].v
23 | # foo("foo", *D["b"].v, d="bar")
24 | foo("foo", D["b"].v[0], D["b"].v[1], d="bar")
   |            ^^^^^^^^^^^
   |
info: rule `not-subscriptable` is enabled by default

error[not-subscriptable]: Cannot subscript object of type `int` with no `__getitem__` method
  --> test.py:24:25
   |
22 | v = D["b"].v
23 | # foo("foo", *D["b"].v, d="bar")
24 | foo("foo", D["b"].v[0], D["b"].v[1], d="bar")
   |                         ^^^^^^^^^^^
   |
info: rule `not-subscriptable` is enabled by default
```

---

_Comment by @carljm on 2026-01-06 02:15_

@phistep I think that your "wrong and misleading in a different way" example would still be covered by #1584 -- we shouldn't map the unpacking to argument `d` when it is provided explicitly.

---
