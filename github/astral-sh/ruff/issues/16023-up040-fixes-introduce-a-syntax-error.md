---
number: 16023
title: UP040 fixes introduce a syntax error
type: issue
state: closed
author: genevieve-me
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-02-07T15:31:26Z
updated_at: 2025-02-07T17:05:19Z
url: https://github.com/astral-sh/ruff/issues/16023
synced_at: 2026-01-07T13:12:16-06:00
---

# UP040 fixes introduce a syntax error

---

_Issue opened by @genevieve-me on 2025-02-07 15:31_

### Description

I recently upgraded a project to python 3.12 (declared in `pyproject.toml`'s `requires-python`) and ran `ruff check --fix  --unsafe-fixes` on the codebase, but got an error with un-applied fixes related to `TypeAlias`es.

Edit: forgot to include, the ruff version is `ruff 0.9.5`.

I isolated the problem to the following example `foo.py`:

```
"""Reuseable type objects."""

import asyncio
from collections.abc import AsyncIterable, Coroutine, Iterable
from typing import Any, TypeAlias

AnyTask: TypeAlias = asyncio.Task[object]
Awaitables: TypeAlias = (
    AsyncIterable[Coroutine[None, None, Any]]
    | Iterable[Coroutine[None, None, Any]]
    | AsyncIterable[AnyTask]
    | Iterable[AnyTask]
)
```

Running `ruff check --fix  --unsafe-fixes foo.py` returns

```
error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at: [snip] ...quoting the contents of `src/utils/foo.py`, the rule codes UP040, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!
```

The pyproject.toml settings are
```
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project]
# snipped
requires-python = ">=3.12"

[tool.ruff]
line-length = 120

[tool.ruff.lint]
select = ["ALL", "D213"]
ignore = ["D203", "D212", "COM812"]

[tool.ruff.lint.pydocstyle]
convention = "google"
```

Thanks!

---

_Comment by @InSyncWithFoo on 2025-02-07 15:58_

The value was deparenthesized when it shouldn't have been:

```python
# Before
Awaitables: TypeAlias = (
    AsyncIterable[Coroutine[None, None, Any]]
    | Iterable[Coroutine[None, None, Any]]
    | AsyncIterable[AnyTask]
    | Iterable[AnyTask]
)

# After
type Awaitables = AsyncIterable[Coroutine[None, None, Any]]
    | Iterable[Coroutine[None, None, Any]]
    | AsyncIterable[AnyTask]
    | Iterable[AnyTask]
```


---

_Label `bug` added by @MichaReiser on 2025-02-07 16:23_

---

_Label `fixes` added by @MichaReiser on 2025-02-07 16:23_

---

_Referenced in [astral-sh/ruff#16026](../../astral-sh/ruff/pulls/16026.md) on 2025-02-07 16:36_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-02-07 16:40_

---

_Closed by @AlexWaygood on 2025-02-07 17:05_

---
