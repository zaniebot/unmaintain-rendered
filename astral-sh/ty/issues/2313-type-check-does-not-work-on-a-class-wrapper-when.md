---
number: 2313
title: Type check does not work on a class wrapper when used as a decorator
type: issue
state: closed
author: owais
labels: []
assignees: []
created_at: 2026-01-03T06:48:45Z
updated_at: 2026-01-04T22:11:02Z
url: https://github.com/astral-sh/ty/issues/2313
synced_at: 2026-01-10T01:51:14Z
---

# Type check does not work on a class wrapper when used as a decorator

---

_Issue opened by @owais on 2026-01-03 06:48_

### Summary

### `ty` fails to catch wrong type of class being decorated

```python
from typing import Protocol, Callable


class NumberProto(Protocol):
    value: int

def set_number_value(override: int) -> Callable[[NumberProto], NumberProto]:
    def wrapper(number: NumberProto):
        number.value = override
        return number 
    return wrapper


@set_number_value(10)
class SomeClass:
    x = 0
```

<img width="570" height="246" alt="Image" src="https://github.com/user-attachments/assets/b3cfd552-5388-4efd-8eef-bfeae48c1734" />

### But works when class is "decorated" without the `@` syntax

`wrapper` expects a class with attribute value of type `int` which `SomeClass` does not satisfy.

```python
from typing import Protocol, Callable


class NumberProto(Protocol):
    value: int


def set_number_value(override: int) -> Callable[[NumberProto], NumberProto]:
    def wrapper(number: NumberProto):
        number.value = override
        return number
    return wrapper


class SomeClass:
    x = 0


SomeClass = set_number_value(10)(SomeClass)
```

<img width="980" height="557" alt="Image" src="https://github.com/user-attachments/assets/7652a3d2-ba85-4f00-8d60-47b0939515de" />



Both snippets do the same thing but `ty` only catches the issue in the non-decorator implementation.

--------

### Version

Python: `3.12` and `3.14`
Ty: `0.0.8`

--------

### pyright works

<img width="1402" height="186" alt="Image" src="https://github.com/user-attachments/assets/83d8ea83-a733-4255-95c1-c6beb6453d15" />


### mypy works as well

<img width="974" height="156" alt="Image" src="https://github.com/user-attachments/assets/dd5508b9-c190-436a-9902-5cd0ee96ee95" />

---

_Comment by @owais on 2026-01-03 07:05_

It works when decorating functions though:

```python
from typing import Protocol, Callable

def add(to_add: int) -> Callable[[Callable[[int], int]], Callable[[int], int]]:
    def wrapper(func: Callable[[int], int]) -> Callable[[int], int]:
        def inner(num: int) -> int:
            return func(num) + to_add
        return inner
    return wrapper

@add(10)
def string(s: str) -> str:
    return s
```

<img width="704" height="520" alt="Image" src="https://github.com/user-attachments/assets/86759859-1bb6-496b-b56f-6a0ccdee9f1e" />

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-04 19:19_

---

_Unassigned @charliermarsh by @charliermarsh on 2026-01-04 19:24_

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-04 20:08_

---

_Added to milestone `Stable` by @carljm on 2026-01-04 20:57_

---

_Closed by @charliermarsh on 2026-01-04 22:11_

---
