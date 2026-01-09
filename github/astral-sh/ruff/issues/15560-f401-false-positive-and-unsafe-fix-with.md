---
number: 15560
title: F401 false positive and unsafe fix with subclassing generics
type: issue
state: closed
author: cibere
labels: []
assignees: []
created_at: 2025-01-17T22:17:07Z
updated_at: 2025-01-17T22:17:48Z
url: https://github.com/astral-sh/ruff/issues/15560
synced_at: 2026-01-07T13:12:16-06:00
---

# F401 false positive and unsafe fix with subclassing generics

---

_Issue opened by @cibere on 2025-01-17 22:17_

I'm encountering a scenario where the F401 rule triggers a false positive when subclassing and providing a generic, with ruff also applying a fix that breaks the typing of the class.

Sample Code: 
```py
from __future__ import annotations
from typing import TYPE_CHECKING, Generic, TypeVar

T = TypeVar("T")

if TYPE_CHECKING:
    from collections.abc import Iterable


class Bar(Generic[T]): ...


class Foo(Bar["Iterable"]): ...
```
and when checking this file with ruff:
```
sample.py:6:33: F401 [*] `collections.abc.Iterable` imported but unused
  |
5 | if TYPE_CHECKING:
6 |     from collections.abc import Iterable
  |                                 ^^^^^^^^ F401
  |
  = help: Remove unused import: `collections.abc.Iterable`

Found 1 error.
[*] 1 fixable with the `--fix` option.
```
Using the `--fix` option changes the code to:
```py
from __future__ import annotations
from typing import TYPE_CHECKING, Generic, TypeVar

T = TypeVar("T")

if TYPE_CHECKING:
    pass


class Bar(Generic[T]): ...


class Foo(Bar["Iterable"]): ...
```
which breaks the typing: `"Iterable" is not defined`

---

_Comment by @cibere on 2025-01-17 22:17_

just saw #9298

---

_Closed by @cibere on 2025-01-17 22:17_

---

_Referenced in [astral-sh/ruff#15860](../../astral-sh/ruff/issues/15860.md) on 2025-02-01 17:05_

---
