---
number: 572
title: B006 (MutableArgumentDefault) should exempt parameters with immutable type annotations
type: issue
state: closed
author: andersk
labels: []
assignees: []
created_at: 2022-11-04T01:35:54Z
updated_at: 2022-11-20T00:46:09Z
url: https://github.com/astral-sh/ruff/issues/572
synced_at: 2026-01-07T13:12:14-06:00
---

# B006 (MutableArgumentDefault) should exempt parameters with immutable type annotations

---

_Issue opened by @andersk on 2022-11-04 01:35_

B006 aims to prevent bugs that arise from mutating the default value of a function’s parameter, with unintended effects on all future calls to that function. But for a parameter that’s annotated with an immutable abstract type such as `Sequence[int]`, mypy already prevents the value from being mutated. So such parameters should be exempt from B006. For example:

```python
from typing import Sequence

def f(a: Sequence[int] = [1, 2, 3]):  # should not raise B006
    for x in a:
        print(x)
```

This was suggested at https://github.com/PyCQA/flake8-bugbear/issues/137#issuecomment-805724850, but [wasn’t implemented](https://github.com/PyCQA/flake8-bugbear/issues/137#issuecomment-808161372) due to flake8-bugbear lacking the machinery to accurately track `typing` imports. Since ruff has that now (#533), perhaps this is easy to implement in ruff.

Types that should be considered immutable:

```python
object
collections.abc.Container[T]
collections.abc.Iterable[T]
collections.abc.Reversible[T]
collections.abc.Sized
collections.abc.Collection[T]
collections.abc.Sequence[T]
collections.abc.Set[T]
collections.abc.Mapping[K, V]
typing.Container[T]
typing.Iterable[T]
typing.Reversible[T]
typing.Sized
typing.Collection[T]
typing.Sequence[T]
typing.AbstractSet[T]  # not typing.Set[T]
typing.Mapping[K, V]
typing.Optional[I]  # where I is any of the above
I | None  # where I is any of the above
typing.Annotated[I, …]  # where I is any of the above
```

---

_Comment by @charliermarsh on 2022-11-04 01:39_

(Agree.)

---

_Label `enhancement` added by @charliermarsh on 2022-11-04 01:39_

---

_Comment by @harupy on 2022-11-04 08:54_

> mypy already prevents the value from being mutated

There might be edge cases where mypy doesn't prevent the value from being mutated.

```python
# a.py

from typing import Sequence


def untyped_func_imported_from_third_party_library(a):
    a.append(1)


def f(a: Sequence[int] = [1, 2, 3]):
    untyped_func_imported_from_third_party_library(a)
    print(a)


f()
f()
```


```
> mypy --version
mypy 0.982 (compiled: yes)

> mypy a.py
Success: no issues found in 1 source file

> python a.py
[1, 2, 3, 1]
[1, 2, 3, 1, 1]
```

---

_Comment by @andersk on 2022-11-04 18:47_

Sure, Python’s gonna Python, so mypy will always be an incomplete solution, although there are options like `--strict` that make it less incomplete.

```console
$ mypy --strict a.py
a.py:4: error: Function is missing a type annotation
a.py:8: error: Function is missing a return type annotation
a.py:9: error: Call to untyped function "untyped_func_imported_from_third_party_library" in typed context
Found 3 errors in 1 file (checked 1 source file)
```

But what I really meant was that, by annotating `a: Sequence[int]`, the programmer has at least declared an *intention* to avoid mutating `a`, and it’s then up to the programmer to enforce this intention, via an appropriate mypy configuration or otherwise.

---

_Referenced in [astral-sh/ruff#821](../../astral-sh/ruff/pulls/821.md) on 2022-11-19 23:45_

---

_Closed by @charliermarsh on 2022-11-20 00:46_

---

_Referenced in [astral-sh/ruff#18441](../../astral-sh/ruff/pulls/18441.md) on 2025-06-03 13:48_

---
