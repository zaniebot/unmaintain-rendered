```yaml
number: 421
title: bug when using overload decorator and numpy arrays
type: issue
state: closed
author: martinResearch
labels:
  - typing semantics
assignees: []
created_at: 2025-05-16T07:30:59Z
updated_at: 2025-05-16T08:18:00Z
url: https://github.com/astral-sh/ty/issues/421
synced_at: 2026-01-12T15:54:23Z
```

# bug when using overload decorator and numpy arrays

---

_@martinResearch_

### Summary

I am getting some error when using a combination of overload and numpy arrays while the code runs and mypy does not complain:

```py
import numpy as np
from typing import overload
 
@overload
def f( a:  np.ndarray, index: int) -> int: ...
@overload
def f( a:  np.ndarray, index: np.ndarray ) -> np.ndarray:  ...
def f(a:  np.ndarray, index: int | np.ndarray) -> np.ndarray|int:
    if index is int:
        return a[index]
    elif isinstance(index,  np.ndarray):
        return a[index]
    else:
        raise TypeError("index must be int or list[int]")


def g(a:  np.ndarray, index:  np.ndarray) -> np.ndarray:
    return a[index]

@overload
def h( a:   list[int], index: int) -> int: ...
@overload
def h( a:   list[int], index: list[int]) -> list[int]:
   ...
def h(a:   list[int], index: int | list[int]) -> list[int]|int:
    if index is int:
        return a[index]
    elif isinstance(index, list):
        return [a[i] for i in index]
    else:
        raise TypeError("index must be int or list[int]")

def main()->None:
    a=np.zeros((10))
    i=np.array([4,5])
    g_a = g(a, i) 
    f_a=f(a,i)
    h_a=h(a,[4,5])
    print(len(h_a)) # works fine
    print(len(g_a)) # works fine
    print(len(f_a)) # Fails: Expected `Sized`, found `int`
    
if __name__ == "__main__":
    main()
```
is that expected? Maybe the problem lies on numpy's side?


### Version

ty==0.0.1a3

---

_Comment by @sharkdp on 2025-05-16 08:16_

> is that expected?

No, thank you for reporting this!

The problem is that we currently do not understand the return type of `np.array([4, 5])`, so `i` ends up being some `@Todo` type, which signals a missing feature (in this case: support for `typing.TypeAlias`). Those `@Todo` types are assignable to everything, which is why we pick the first `f` overload here, returning `int` instead of `np.ndarray`.

---

_Closed by @sharkdp on 2025-05-16 08:17_

---

_Label `typing semantics` added by @sharkdp on 2025-05-16 08:18_

---
