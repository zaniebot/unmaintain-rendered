---
number: 2013
title: "No error reported for incorrect arguments passed to function generic over a `ParamSpec`"
type: issue
state: closed
author: WillDuke
labels:
  - bug
  - generics
assignees: []
created_at: 2025-12-17T14:16:39Z
updated_at: 2026-01-08T07:09:49Z
url: https://github.com/astral-sh/ty/issues/2013
synced_at: 2026-01-10T01:51:14Z
---

# No error reported for incorrect arguments passed to function generic over a `ParamSpec`

---

_Issue opened by @WillDuke on 2025-12-17 14:16_

### Summary

`ty` no longer raises a diagnostic on this code:

```python
from typing import ParamSpec, TypeVar, Callable

P = ParamSpec("P")
T = TypeVar('T')

def fn(p1: int, p2: int):
    ...

def submit(fn: Callable[P, T], *args: P.args, **kwargs: P.kwargs):
    ...

submit(fn, p1=1, p2="wrong")
```

Here's the [link](https://play.ty.dev/74ba704a-07ae-4e6a-ba36-b0fae27b8a10) to the playground with the above snippet.

Here's a motivating example where this comes up ([link](https://play.ty.dev/51c02e59-ff26-4904-a6b2-b73326bd6e2c)).

```python
from concurrent.futures import ThreadPoolExecutor



def fn(p1: int, p2: int):
    ...

with ThreadPoolExecutor() as executor:
    executor.submit(fn, p1=1, p2="wrong")
```

I was originally going to submit this issue because I noticed that the diagnostic gave (what I think is) the correct error but highlighted p1 instead of p2, but as of today the diagnostic no longer appears at all. 

Here's the original diagnostic issue using ty as built into Zed, updated yesterday. (Notice the 1 is underlined instead of "wrong".)

<img width="949" height="133" alt="Image" src="https://github.com/user-attachments/assets/e30c99ee-b067-46f8-9b76-82a0602c1d8b" />

### Version

main (2a61fe235)

---

_Label `bug` added by @AlexWaygood on 2025-12-17 14:37_

---

_Label `generics` added by @AlexWaygood on 2025-12-17 14:37_

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-17 14:38_

---

_Comment by @AlexWaygood on 2025-12-17 14:38_

Thanks for the report! I'm not _totally_ sure that this ever worked -- I think the error you were seeing before was an unrelated bug 😆

But it definitely _should_ work, agreed!

---

_Comment by @WillDuke on 2025-12-17 14:55_

Thanks, the original error fooled me into thinking this was already supported! 

---

_Renamed from "ty no longer reports type mismatches in a ParamSpec" to "No error reported for incorrect arguments passed to function generic over a `ParamSpec`" by @AlexWaygood on 2025-12-17 16:16_

---

_Comment by @AlexWaygood on 2025-12-17 16:17_

@dhruvmanila, is this related to the last bullet point in https://github.com/astral-sh/ty/issues/1861?

---

_Comment by @dhruvmanila on 2025-12-23 04:50_

Interesting, I think this was working before even though the diagnostic position is incorrect:

```console
$ uvx "ty@0.0.1a35" check .
Installed 1 package in 6ms
error[invalid-argument-type]: Argument to function `submit` is incorrect
  --> main.py:13:12
   |
13 | submit(fn, p1=1, p2="wrong")
   |            ^^^^ Expected `int`, found `Literal["wrong"]`
   |
info: Function defined here
  --> main.py:10:5
   |
10 | def submit(fn: Callable[P, T], *args: P.args, **kwargs: P.kwargs): ...
   |     ^^^^^^                     ------------- Parameter declared here
   |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

But, it stopped working in `0.0.2`, it might be related to the generic `Callable` PR. I'll take a look.

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-12-23 04:50_

---

_Comment by @dhruvmanila on 2025-12-24 06:52_

I tried a few combinations of the original snippet and it seems to only occur when the `Callable` has a generic return type `T` and that the passed in function does not have an annotated return type. For example:

```py
from typing import Callable

def fn(a: int): ...

def submit[**P, T](fn: Callable[P, T], *args: P.args, **kwargs: P.kwargs): ...

submit(fn, a="a")
```

I think I see what the issue is but I'm not exactly sure what the solution should be. For a `Callable[P, T]` we'd create two constraints corresponding to `P` and `T` and combine them using `and` [here](https://github.com/astral-sh/ruff/blob/81f34fbc8e404cd26fa40ee86a339947dce7617c/crates/ty_python_semantic/src/types/signatures.rs#L486). For the above example, this would be:

```py
((a: int) ≤ P@submit)  # for P
never                  # for T
```

And, the fact that it's `never` for `T` means that the `and` call would result in both `P` and `T` be never satisfied so we loose the paramspec constraint as well from the specialization:

```py
# should be [(a: int), Unknown]
specialization: [(...), Unknown]
```

Using `or` between `P` and `T` would resolve this but I'm not sure if that's the correct approach. I'd want to first understand why it's `and` in the first place.

---

_Comment by @carljm on 2025-12-27 00:34_

Definitely seems like a @dcreager question, but my thoughts just on reading this are 1) `and` instead of `or` seems correct here? We need both the return type and the parameters to match, not just one or the other. And 2) what seems wrong here is the `never` for `T` -- `T` should be unconstrained here (or constrained to `Unknown`, which is equivalent?), there should be no constraints on its type that would cause the overall `and` to fail to be satisfied.

---

_Unassigned @dhruvmanila by @dhruvmanila on 2026-01-07 08:36_

---

_Closed by @carljm on 2026-01-07 17:18_

---

_Comment by @WillDuke on 2026-01-07 21:00_

Thanks for the fix! Incidentally, #22425 resurfaced the earlier issue from 0.0.1a35 where the diagnostic highlights the wrong parameter. Is that worth a separate issue? 

---

_Comment by @dhruvmanila on 2026-01-08 07:09_

> Thanks for the fix! Incidentally, #22425 resurfaced the earlier issue from 0.0.1a35 where the diagnostic highlights the wrong parameter. Is that worth a separate issue?

https://github.com/astral-sh/ty/issues/2198

---
