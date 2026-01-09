---
number: 6849
title: "TCH002 false positive with `singledispatch`"
type: issue
state: closed
author: flying-sheep
labels:
  - bug
assignees: []
created_at: 2023-08-24T10:15:55Z
updated_at: 2023-12-01T03:04:59Z
url: https://github.com/astral-sh/ruff/issues/6849
synced_at: 2026-01-07T13:12:15-06:00
---

# TCH002 false positive with `singledispatch`

---

_Issue opened by @flying-sheep on 2023-08-24 10:15_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

[Playground link](https://play.ruff.rs/0513d9af-edc8-439f-be51-b6819393139b)

[`functools.singledispatch`](https://docs.python.org/3/library/functools.html#functools.singledispatch)’s `register` decorator can take a function with the first argument being type-annotated. That type annotation will then be used to dispatch.

The following code should therefore be correct with all rules active, however it claims rule violations on the annotated lines.

```py
"""Test module."""
from __future__ import annotations

from functools import singledispatch
from typing import TYPE_CHECKING

from numpy import asarray  # TCH002: Move third-party import `numpy.typing.ArrayLike` into a type-checking block
from numpy.typing import ArrayLike  # TCH002: Move third-party import `scipy.sparse.spmatrix` into a type-checking block
from scipy.sparse import spmatrix

if TYPE_CHECKING:
    from numpy import ndarray


@singledispatch
def to_array_or_mat(a: ArrayLike | spmatrix) -> ndarray | spmatrix:
    """Convert arg to array or leaves it as sparse matrix."""
    msg = f"Unhandled type {type(a)}"
    raise NotImplementedError(msg)

@to_array_or_mat.register
def _(a: ArrayLike) -> ndarray:
    return asarray(a)

@to_array_or_mat.register
def _(a: spmatrix) -> spmatrix:
    return a
```

---

_Comment by @flying-sheep on 2023-08-24 11:07_

@ivirshup support for pydantic-like and attrs-like patterns can already be achieved via [`flake8-type-checking.runtime-evaluated-base-classes`](https://beta.ruff.rs/docs/settings/#flake8-type-checking-runtime-evaluated-base-classes) and [`flake8-type-checking.runtime-evaluated-decorators`](https://beta.ruff.rs/docs/settings/#flake8-type-checking-runtime-evaluated-decorators), respectively

---

_Label `bug` added by @charliermarsh on 2023-08-24 13:25_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-30 20:20_

---

_Referenced in [astral-sh/ruff#8934](../../astral-sh/ruff/pulls/8934.md) on 2023-12-01 02:05_

---

_Closed by @charliermarsh on 2023-12-01 03:05_

---
