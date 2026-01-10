---
number: 2095
title: "lambdas returning `Unknown` type result in wrong result type in generic function calls (e.g. `map`)"
type: issue
state: closed
author: karlicoss
labels: []
assignees: []
created_at: 2025-12-18T23:39:23Z
updated_at: 2026-01-08T19:22:23Z
url: https://github.com/astral-sh/ty/issues/2095
synced_at: 2026-01-10T01:48:23Z
---

# lambdas returning `Unknown` type result in wrong result type in generic function calls (e.g. `map`)

---

_Issue opened by @karlicoss on 2025-12-18 23:39_

### Summary

https://play.ty.dev/aa9c704f-34b4-4290-8b91-efa576696917

```py
lstr: list[str] = ['a', 'bb', 'ccc']

def str2int_fun(s: str) -> int:
    return int(s)

reveal_type(str2int_fun)  # reveals def str2int_fun(s: str) -> int
ints_fun = map(str2int_fun, lstr) 
reveal_type(ints_fun)  # reveals map[int] (as expected)
```

Above works fine -- now let's define the same function as a lambda
```py
str2int_lam = lambda s: int(s)

reveal_type(str2int_lam)  # reveals (s) -> Unknown (both on 0.0.3 and 0.0.1a35)
ints_lam = map(str2int_lam, lstr)
reveal_type(ints_lam)  # reveals map[Unknown | str] on 0.0.3; map[Unknown] on 0.0.1a35
```

`map[Unknown | str]` is odd! Seems like something could be wrong with the type variable resolution and it somehow grabbed the type of iterable (list) we're mapping over?

Thought it could have to do with overloads, but if I define my own simple map function still same behaviour:
```py
from typing import Callable
def map2[T, R](f: Callable[[T], R], l: list[T]) -> list[R]:
    raise RuntimeError("whatever")

reveal_type(map2(str2int_fun, lstr))  # reveals list[int]
reveal_type(map2(str2int_lam, lstr))  # reveals list[Unknown | str]
````

I checked whether it was somehow an issue with functions returning `Unknown`, but seems like it works correctly in that case (at in terms of least not gaining `| str`!) https://play.ty.dev/9d801483-8fb9-44fe-9943-520e2f12fb53

Also works fine if we do something like `fun = cast(Callable[[str], Unknown], None)`

(hopefully not a dupe 😅)

### Version

ty 0.0.3 / latest main

---
