```yaml
number: 848
title: Differentiate between identically named (but distinct) types in diagnostics
type: issue
state: closed
author: Andre-Medina
labels:
  - diagnostics
assignees: []
created_at: 2025-07-18T04:49:46Z
updated_at: 2025-12-18T13:44:59Z
url: https://github.com/astral-sh/ty/issues/848
synced_at: 2026-01-10T01:53:59Z
```

# Differentiate between identically named (but distinct) types in diagnostics

---

_Issue opened by @Andre-Medina on 2025-07-18 04:49_

## Context

When you have two types of the same name from different packages/ parts of the code base `Ty` will only give the error based on type name, and not its dir, leading to ambiguous errors.

I suggest adding some extra logic to be able to easier differentiate between identically named types, so in the case that it finds this issue, it appends something about the dir of the type.

## Example

Say I have a function which expects a `polars` dataframe but a `pandas` dataframe is passed:
```
import polars as pl
import pandas as pd

def polars_function(df: pl.DataFrame) -> pl.DataFrame:
    return df

df_pd = pd.DataFrame()
polars_function(df_pd)
```

Will receive this ambiguous error:
<img width="939" height="339" alt="Image" src="https://github.com/user-attachments/assets/cb569638-b4f5-4313-b90e-68534650f6f6" />

## Solution

Would be very helpful if `ty` noticed that `DataFrame` and `DataFrame` are the same name and then appended the package name/ dir as follows:
```
 --> test_file.py:8:17
  |
7 | df_pd = pd.DataFrame()
8 | polars_function(df_pd)
  |                 ^^^^^ Expected `polars.DataFrame`, found `pandas.DataFrame`
```

### Version

ty 0.0.1-alpha.14

---

_Label `question` added by @Andre-Medina on 2025-07-18 04:49_

---

_Label `question` removed by @AlexWaygood on 2025-07-20 17:56_

---

_Label `diagnostics` added by @AlexWaygood on 2025-07-20 17:56_

---

_Renamed from "Differentiate between identically named types" to "Differentiate between identically named (but distinct) types in diagnostics" by @AlexWaygood on 2025-07-20 17:57_

---

_Comment by @AlexWaygood on 2025-08-07 09:47_

Here's another great example from #950:

```py
class A:
    class B:
        pass

class C:
    class B:
        pass


a: A.B = C.B()
```

---

_Comment by @leandrobbraga on 2025-08-10 23:18_

I'l add one more example:

```python 
# t.py
from __future__ import annotations


def my_function() -> A:
    class A:
        pass

    return A()


class A:
    pass


a: A = my_function()
```

In this case `ty`:
1. it's not differentiating the class names
2. it's resolving the return type to the global class instead of the function-local

Is 2. the expected behavior? Should we open a new issue? 

#### mypy

```console
t.py:8: error: Incompatible return value type (got "t.A@5", expected "t.A")  [return-value]
```

From my tests the number in `A@5` comes from the line number.

#### ty

```console
error[invalid-return-type]: Return type does not match returned value
 --> teste.py:4:19
  |
4 | def my_function() -> A:
  |                   - Expected `A` because of return type
5 |     class A:
6 |         pass
7 |
8 |     return A()
  |            ^^^ expected `A`, found `A`
  |
info: rule `invalid-return-type` is enabled by default
```

I believe we should have
error: [invalid-assignment] "Object of type `t.<local in my_function>.A` is not assignable to `t.A`"


---

_Comment by @AlexWaygood on 2025-08-11 13:27_

> ```py
> # t.py
> from __future__ import annotations
> 
> 
> def my_function() -> A:
>     class A:
>         pass
> 
>     return A()
> 
> 
> class A:
>     pass
> 
> 
> a: A = my_function()
> ```
> 
> In this case `ty`:
> 
>     1. it's not differentiating the class names
>     2. it's resolving the return type to the global class instead of the function-local
> 
> Is 2. the expected behavior? Should we open a new issue?

(2) is expected behaviour here. It matches Python's name-resolution rules at runtime: names in function annotations are resolved at function-definition time (before any of the body of the function has been executed), so the `A` in the function return annotation must refer to the global name `A` rather than the function local `A`. It also matches what mypy and pyright do here.

Mypy seems to do much better than [pyright](https://pyright-play.net/?pythonVersion=3.13&strict=true&enableExperimentalFeatures=true&code=MQAgLgdADgngUAMwE4HsC2ID6mEFcy5ICm2IAlmlCkmCAIYB2DKYdYZKDAznLwCZEEINDBy4GAY3acAFAEoQAWgB8IAIIAuOCB0gJAGzpcu6rbvMgoRntt3ECSBuvm84B66ds6rx13Q3qIAC8wqJ4ktIMLkA) here when it comes to distinguishing between the function-local `A` and the global `A` in its diagnostics. However, I still don't consider the `A@5` notation to be very user-friendly -- where does `5` come from?? Pyrefly seems to do a similar thing to mypy.

What if we did something similar to the repr given to these classes at runtime? E.g.

```pycon
>>> def f():
...     class A: ...
...     return A()
...     
>>> f()
<__main__.f.<locals>.A object at 0x100e10c20>
```

In the example above, the fully qualified name of `A` could be something like `t.<locals of function 'f'>.A`. It's a bit wordy but it has the advantage of being clear (I think?), and it's similar to what Python users will be used to from CPython at runtime

---

_Comment by @Andre-Medina on 2025-08-11 23:38_

>  It's a bit wordy but it has the advantage of being clear

As this is an edge case of an edge case, definitely worth the tradeoff for the sake of clarity. Could slightly simplify mimicking the above formatting of `t.f.<locals>.A`. But much better than a random `@5` (which is probably from the line number)

---

_Comment by @leandrobbraga on 2025-08-12 01:29_

I ended implementing the <locals of function 'f'> but I couldn't come up with a test that trigger it. 

---

_Closed by @carljm on 2025-08-27 20:46_

---

_Comment by @carljm on 2025-08-27 21:01_

> I ended implementing the <locals of function 'f'> but I couldn't come up with a test that trigger it.

I added a test that triggers this. It requires doing the assignment inside the function that defines the nested class.

Re-opening this issue since we still need to support functions, and nested types.

---

_Reopened by @carljm on 2025-08-27 21:01_

---

_Closed by @AlexWaygood on 2025-08-28 09:27_

---

_Reopened by @carljm on 2025-08-28 14:40_

---

_Comment by @carljm on 2025-08-28 17:06_

Something that occurred to me today: although in general I think our fully-qualified-name approach is clearer than mypy's `@line-number` approach, there are cases that are still ambiguous with fully-qualified names, and unambiguous with mypy's version: https://play.ty.dev/627d5c8a-ab85-43bd-87d3-025eaaf1486e

I think I'm OK sticking with our approach for now and seeing if that kind of weird case ever occurs in the real world. It wouldn't be a hard change to make if we ever decide to switch to mypy's approach.

---

_Comment by @leandrobbraga on 2025-08-28 17:13_

> Something that occurred to me today: although in general I think our fully-qualified-name approach is clearer than mypy's `@line-number` approach, there are cases that are still ambiguous with fully-qualified names, and unambiguous with mypy's version: https://play.ty.dev/627d5c8a-ab85-43bd-87d3-025eaaf1486e
> 
> I think I'm OK sticking with our approach for now and seeing if that kind of weird case ever occurs in the real world. It wouldn't be a hard change to make if we ever decide to switch to mypy's approach.

It's not exclusive to function local, it seems to be a very niche situation where both fully qualified name and the regular name collides.
https://play.ty.dev/302166b4-a93c-443f-a731-c7fc46d2fa36

---

_Comment by @spaceone on 2025-12-18 11:44_

`mypy` doesn't detect this one:

```
$ ty check t.py 
t.py:7:9: error[invalid-method-override] Invalid override of method `__eq__`: Definition is incompatible with `Foo.__eq__`
Found 1 diagnostic
$ cat t.py
```
```python
class Foo:
    def __eq__(self, other: object) -> bool:
        return False


class Bar(Foo):
    def __eq__(self, value: object) -> bool:
        return False
```

---

_Comment by @AlexWaygood on 2025-12-18 12:57_

@spaceone that's unrelated to this issue. I gave an explanation of why that error occurs in https://github.com/astral-sh/ty/issues/2000#issuecomment-3665052994, however

---

_Comment by @AlexWaygood on 2025-12-18 13:44_

Following https://github.com/astral-sh/ruff/pull/22019 being merged, I think we can close this now. There are probably still some diagnostics where we're not using the fully qualified names of types in situations where we should be, but let's open targeted bug reports regarding those specific diagnostics when and if we encounter them

---

_Closed by @AlexWaygood on 2025-12-18 13:44_

---
