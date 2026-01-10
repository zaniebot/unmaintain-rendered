```yaml
number: 1505
title: ty fails to enforce invariance and covariance rules for generic containers (List vs Sequence)
type: issue
state: closed
author: FHaggs
labels: []
assignees: []
created_at: 2025-11-09T12:16:40Z
updated_at: 2025-11-09T16:53:06Z
url: https://github.com/astral-sh/ty/issues/1505
synced_at: 2026-01-10T02:06:25Z
```

# ty fails to enforce invariance and covariance rules for generic containers (List vs Sequence)

---

_Issue opened by @FHaggs on 2025-11-09 12:16_

### Summary

https://play.ty.dev/54af2346-91e3-407f-a99f-bad9b19360ef

ty does not flag violations of Python’s type variance rules when using list and Sequence type hints.

Specifically:

List should be invariant

Sequence should be covariant

However, ty currently accepts code that passes list[Dog] where list[Animal] is expected which breaks type safety guarantees that every other checker (Pyright) enforces. Check the reference for more information.

Also the other way around does not flag an error.

```py
def imutable_thing(arr: list[Animal]) -> int:
    return len(arr)
```

These should not accept a list[Dog], the function signature should be arr: Sequence[Animal]. That would be ok.

References:
https://peps.python.org/pep-0484/#covariance-and-contravariance

---

_Comment by @AlexWaygood on 2025-11-09 16:50_

Thanks for the report!

The issue here isn't that ty doesn't understand variance -- we do! Here's your initial snippet, and you're correct that we don't emit any diagnostics on it:

```py
from typing import Sequence

class Animal:
    pass

class Dog(Animal):
    pass


def imutable_thing(arr: Sequence[Animal]) -> int:
    return len(arr)

def mutable(arr: list[Animal]):
    arr.append(Animal())


a = [Animal(), Animal()]
d = [Dog(), Dog()]

res = imutable_thing(d)  # ✅ Should be fine (Sequence is covariant)
mutable(a)               # ✅ OK
mutable(d)               # ❌ Should FAIL — List is invariant
```

But if I apply this diff to your snippet...

```diff

```py
  from typing import Sequence

  class Animal:
      pass

  class Dog(Animal):
      pass


  def imutable_thing(arr: Sequence[Animal]) -> int:
      return len(arr)

  def mutable(arr: list[Animal]):
      arr.append(Animal())


- a = [Animal(), Animal()]
- d = [Dog(), Dog()]
+ a: list[Animal] = [Animal(), Animal()]
+ d: list[Dog] = [Dog(), Dog()]

  res = imutable_thing(d)  # ✅ Should be fine (Sequence is covariant)
  mutable(a)               # ✅ OK
  mutable(d)               # ❌ Should FAIL — List is invariant
```

Then ty does indeed emit the diagnostic you're expecting:

```
error[invalid-argument-type]: Argument to function `mutable` is incorrect
  --> foo.py:22:9
   |
20 | res = imutable_thing(d)  # ✅ Should be fine (Sequence is covariant)
21 | mutable(a)               # ✅ OK
22 | mutable(d)               # ❌ Should FAIL — List is invariant
   |         ^ Expected `list[Animal]`, found `list[Dog]`
   |
info: Function defined here
  --> foo.py:13:5
   |
11 |     return len(arr)
12 |
13 | def mutable(arr: list[Animal]):
   |     ^^^^^^^ ----------------- Parameter declared here
14 |     arr.append(Animal())
   |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

The issue is that without the explicit annotations, ty doesn't infer the type of `a` as `list[Animal]` (it infers `list[Animal | Unknown]`) and doesn't infer the type of `d` as `list[Dog]` (it infers `list[Dog | Unknown]`).

Correctly inferring the type of unannotated collection literals is a surprisingly hard problem. We know we need to do better here; the `| Unknown` union is a temporary measure to avoid false positives. One such false positive can be demonstrated from your example here: an equally valid inference for the `d` variable would be `list[Animal]` rather than `list[Dog]` -- no type checker has an issue if you provide `list[Animal]` as the annotation for that variable:

```py
from typing import Sequence

class Animal:
    pass

class Dog(Animal):
    pass


def imutable_thing(arr: Sequence[Animal]) -> int:
    return len(arr)

def mutable(arr: list[Animal]):
    arr.append(Animal())


a: list[Animal] = [Animal(), Animal()]
d: list[Animal] = [Dog(), Dog()]

res = imutable_thing(d)  # ✅ Should be fine (Sequence is covariant)
mutable(a)               # ✅ OK
mutable(d)               # ❌ Should FAIL — List is invariant

```

- [Mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=9382f8d13f715a0019eef13ff97b2aba)
- [Pyright](https://pyright-play.net/?pyrightVersion=1.1.405&strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoDKApgI4CuRKAxkQLABQDVANgIYDO7UAgilq8wBcDKKKgIO7BkzacoAETBoAFL37MAlMPpjxk6Y3oATIsEwQyMVgCNmRAPowAFqhWsQIQYVIVqRANpqEAIAuhpQALQAfJgoMNq6IEQwZCAoUHYoyu4gGgYmZhZWtkTZHl7MSOwwgXzBzGEJYjkAdKwICJRGqnUCyhp5hgysFVU1QaFQALxQter9ADQ8vcz9IQxGo9Vz9SHTs4oqGkuHawZJXDNYljZ2ji7oykbhUADEUICg5IROYGTMRlBrEQoMBUMDlMRyJQaJguFQwAA3dxIVhxQZFW6lVgvXS43TvL4AeQA0gwMSUnji8Xj3oAZcm%2Bv3%2BUAAYtwAJIAGSggBQCKAcsaw2JIkAouIGIA)
- [Pyrefly](https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeS4ATrgLYAEALqcROgOZ0Q3G6UN0BlGAEcArjHQBjGAB10cyVFRw4dAILouqKIjl19dYsrhyFSlXQAiuNgAoNWqAEpd6A4eOn5WGGE41RBlRsWAB9BgALVjtUSkpEQRFxKRgAbQcabQBdJzoAWgA%2BTnQGV3dKGAZRSjdYdFtYyicvTF86AKCQmAa4hKgIOAZ0zUyoHLKDRsJUYmIJTHsR7VsnZu85VD6BoYzsugBeOmHHFYAadSWoFay5TC3B49Gsg6PrOydzt%2BuvCtVDrkCwTCkWitkwuToAGI6IBQckEEVwoigmDo2BgdEg6HRtiEYgk0k4qkkuAAbrEIBgGGsOkDuqgIe5Ge5oXCAPIAaTkNK6YIZTKZ0MAMuTwxHIugAMTUAEkADJ0QAoBHQZdtCcUyZQKSUvCBTiBAtA4CRyIgQNCAKoMaAQJgY0RSS24dAmbytPxgXiZBihdCiGhoyi2fAJVhU-JFQbxPQGCpVGoYmQgAByvv9CWA%2BAAvgm5DqQGQKmAoKRCAxaFAKNCAAqkAtFuhoLB4fB0YnoSBsaqoB3oQhyaFCdERBgMYhwRAAenH%2Bd8RcIvDY44k48wuEkcHHrfbne744xvDoqDJ0FpLcdW8oXYgjrouGI3cNcjIkUdeRJMEocCvbkOCYAzIQAEYACZs3QEAM11VBJEtN9xWgGAKAbHACCNcCgA)

So we can't know exactly what the user intended without an explicit annotation. In situations like this, other type checkers either demand type annotations or make (what we consider to be) unwarranted assumptions regarding the user's intent (for example, solving the type as `list[Dog]` even though `list[Animal]` would be an equally valid way to solve the type and would avoid a type error being emitted later on in the scope). We hope eventually to be able to use constraints and bidirectional inference to do better than other type checkers in this area -- but this is still some way off.

See https://github.com/astral-sh/ty/issues/1473 for more detail on our plans in this area.

---

_Closed by @AlexWaygood on 2025-11-09 16:50_

---

_Comment by @FHaggs on 2025-11-09 16:53_

Thank you for the explanation

---
