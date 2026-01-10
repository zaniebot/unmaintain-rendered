---
number: 560
title: Support narrowing on tuple length checks
type: issue
state: open
author: MatthewMckee4
labels:
  - narrowing
assignees: []
created_at: 2025-06-01T12:21:07Z
updated_at: 2026-01-09T15:24:40Z
url: https://github.com/astral-sh/ty/issues/560
synced_at: 2026-01-10T01:48:23Z
---

# Support narrowing on tuple length checks

---

_Issue opened by @MatthewMckee4 on 2025-06-01 12:21_

From the typing spec: https://typing.python.org/en/latest/spec/tuples.html#type-compatibility-rules, we currently do not support this.
```py
def func(val: tuple[int] | tuple[str, str] | tuple[int, *tuple[str, ...], int]):
    if len(val) == 1:
        # Type can be narrowed to tuple[int].
        reveal_type(val)  # tuple[int]

    if len(val) == 2:
        # Type can be narrowed to tuple[str, str] | tuple[int, int].
        reveal_type(val)  # tuple[str, str] | tuple[int, int]

    if len(val) == 3:
        # Type can be narrowed to tuple[int, str, int].
        reveal_type(val)  # tuple[int, str, int]
```
Im happy to take this on but i think that we may need to wait until we understand `tuple[int, *tuple[str, ...], int]`

---

_Label `narrowing` added by @AlexWaygood on 2025-06-01 15:40_

---

_Renamed from "Supoprt narrowing on tuple length checks" to "Support narrowing on tuple length checks" by @sharkdp on 2025-06-03 07:48_

---

_Comment by @MatthewMckee4 on 2025-06-17 12:16_

After reading over #639, i had another thought about this, and with a tuple subclass that overrides the `__len__` method, isn't this unsound?

From the spec

> The length of a tuple at runtime is immutable, so it is safe for type checkers to use length checks to narrow the type of a tuple:

This also seems like an incorrect conclusion.

---

_Comment by @AlexWaygood on 2025-06-17 12:28_

We intend to emit an error on tuple subclasses that override `__len__` or `__bool__` to guard against the unsoundness here. I agree that if you don't do that but you do allow tuple subclasses in general, this narrowing would be unsound!

---

_Comment by @AlexWaygood on 2025-06-27 13:13_

I propose we implement this by making the following changes:

1. For a fixed-length tuple, we synthesize a `__len__` method with a precise return type. For example, `tuple[int, int]` would have a synthesized `__len__` method that returns `Literal[2]`.
2. We add a protocol like this to the `ty_extensions` module:
   ```py
   from typing import Sized, Protocol

   class ExactlySized[T: int](Sized):
       def __len__(self) -> T: ...
   ```
3. We intersect with that protocol on seeing a length check, e.g.:
   ```py
   def f(x: object):
       if len(x) == 4:
           reveal_type(x)  # revealed: ExactlySized[Literal[4]]
   ```
4. We implement the Liskov Substitution Principle, such that this would be an error:
   ```py
   from typing import Literal

   class Foo:
       def __len__(self) -> Literal[5]: ...

   class Bar(Foo):
       def __len__(self) -> int:
           return 42
   ```

   and therefore (due to the synthesized `__len__` method for fixed-length tuples implemented as change (1)), this would be an error:

   ```py
   class Foo(tuple[int, int]):
       def __len__(self) -> int:
           return 42
   ```

5. We implement disjointness for protocols vs nominal types (this is being done in https://github.com/astral-sh/ruff/pull/18847, but not yet for protocols with method members) so that we understand that `tuple[int, int]` is disjoint from `ExactlySized[Literal[4]]`, because its `__len__` method returns a type disjoint from the return type of `ExactlySized[Literal[4]].__len__`. This will enable narrowing such as this:

   ```py
   def f(x: tuple[int] | tuple[int, str] | tuple[bytes, bytes, bool]):
       if len(x) == 2:
           # => (tuple[int] & ExactlySized[Literal[2]]) | (tuple[int, str] & ExactlySized[Literal[2]]) | (tuple[bytes, bytes, bool] & ExactlySized[Literal[2]])
           # => Never | tuple[int, str] | Never
           # => tuple[int, str]
           reveal_type(x)
   ```

We can also synthesize precise methods for `__getitem__` and `__bool__` on non-homogeneous tuple types, so that inheriting from them is naturally also sound as a result of our Liskov implementation.

Doing things in this way has the advantage that we will do narrowing for other classes that are known to have fixed lengths too, not just tuples, e.g.

```py
from typing import Literal

class Foo:
    def __len__(self) -> Literal[3]:
        return 3

class Bar:
    def __len__(self) -> Literal[4]:
        return 4

def f(x: Foo | Bar):
    if len(x) == 3:
        reveal_type(x)  # Foo
```

---

_Comment by @MatthewMckee4 on 2025-07-22 18:16_

@AlexWaygood Do you think this is ready to go after https://github.com/astral-sh/ruff/pull/19289?

---

_Comment by @AlexWaygood on 2025-07-22 18:40_

not yet, we don't support protocols with method members properly yet. I'm working on that this week.

---

_Comment by @MatthewMckee4 on 2025-07-22 19:24_

Does this mean we can then make a `ExactlySized` protocol?

---

_Comment by @MatthewMckee4 on 2025-07-22 19:27_

And will that then allow us to synthesise a `ExactlySized`, which we can then use to narrow

---

_Comment by @inigohidalgo on 2025-08-15 14:32_

Some of this discussion is going over my head but I came across this issue searching for the problem I'm having and I wonder if this fix would also solve my case. If not I will open a separate issue and hide my comment.

```python
drops: list[tuple[int, int, str]] = []
drops.append((3, 4, "a", "a"))
```

```bash
>>> uvx ty check
All checks passed!
```

```bash
>>> uvx mypy src/
src/obsidian_chunker/io/parser.py:20: error: Argument 1 to "append" of "list" has incompatible type "tuple[Any, Any, Any | str, str]"; expected "tuple[int, int, str]"  [arg-type]
```

`mypy` raises the expected error for an incompatible `.append` but `ty` does not catch the incompatible length from the type hint.



---

_Comment by @AlexWaygood on 2025-08-15 16:13_

@inigohidalgo thanks for the report! That's unrelated to this issue, however -- see https://github.com/astral-sh/ty/issues/543 and/or https://github.com/astral-sh/ty/issues/168 and/or https://github.com/astral-sh/ty/issues/136

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-08-22 13:09_

---

_Added to milestone `GA` by @carljm on 2025-11-07 14:15_

---

_Assigned to @oconnor663 by @oconnor663 on 2025-11-14 15:21_

---

_Comment by @spaceone on 2025-12-18 12:24_

same issue for me:

```python
def foo(*parts: str | tuple[str, str] | tuple[str, str, int]) -> None:
    cmp: list[list[tuple[str, str, int]]] = []
    for part in parts:
        if isinstance(part, str):
            pass
        elif isinstance(part, tuple) and len(part) == 3:  # noqa: PLR2004
            cmp.append([part])
        elif isinstance(part, tuple) and len(part) == 2:  # noqa: PLR2004
            pass
```
```sh
$ ty check t.py 
t.py:7:25: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `list[tuple[str, str, int]]`, found `list[tuple[str, str, int] | tuple[str, str]]`
Found 1 diagnostic
```


---

_Unassigned @oconnor663 by @oconnor663 on 2026-01-09 15:24_

---
