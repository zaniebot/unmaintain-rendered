---
number: 9904
title: "Rule UP040 incorrect fix when `TypeVar` is used multiple times"
type: issue
state: closed
author: ostr00000
labels:
  - bug
assignees: []
created_at: 2024-02-09T02:52:27Z
updated_at: 2024-02-09T14:02:43Z
url: https://github.com/astral-sh/ruff/issues/9904
synced_at: 2026-01-10T01:22:49Z
---

# Rule UP040 incorrect fix when `TypeVar` is used multiple times

---

_Issue opened by @ostr00000 on 2024-02-09 02:52_

## Initial code
This is code in stub file `decorators.pyi`:
```python
import logging
from collections.abc import Callable
from typing import TypeAlias, TypeVar

_BaseFun = TypeVar('_BaseFun', bound=Callable)
_Decorator: TypeAlias = Callable[[_BaseFun], _BaseFun]

def logCurrentTask(
    *, logger: logging.Logger | None = None, msg: str = ''
) -> _Decorator: ...

```
## Ruff command
After running ruff with command:
```bash
ruff check ./decorators.pyi --isolated --select UP040 --target-version py312 --fix
```
`TypeVar` is replaced to generic type alias. 

## Code after fix
The code after change is:
```python
import logging
from collections.abc import Callable
from typing import TypeAlias, TypeVar

_BaseFun = TypeVar('_BaseFun', bound=Callable)
type _Decorator[_BaseFun: Callable, _BaseFun: Callable] = Callable[[_BaseFun], _BaseFun]

def logCurrentTask(
    *, logger: logging.Logger | None = None, msg: str = ''
) -> _Decorator: ...
```

## What is wrong
Type alias `_Decorator` has two declarations with same name `_BaseFun` which is incorrect syntax.
The [PEP 695](https://peps.python.org/pep-0695/#generic-type-alias) states:

> Type parameter names within a generic class, function, or type alias must be unique within that same class, function, or type alias


## Possible fix
Need to check for duplicates.



## Ruff version
```bash
ruff --version
```
`ruff 0.2.1`

## Additional thinking

Not required to this issue, but it may be worth to notify user about remaining, old `TypeVar` or even remove it when rule UP040 is applying.
Although the variable is not used anymore, it remains in code, possibly unnoticed (I assume that currently only stubs are processed for this rule, but in future it may change).
The old object remains in global scope, which is not detected by most linters/type checkers.

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Label `bug` added by @charliermarsh on 2024-02-09 02:53_

---

_Comment by @charliermarsh on 2024-02-09 02:53_

Interesting, thank you for the clear issue! We can fix this.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-09 03:10_

---

_Referenced in [astral-sh/ruff#9905](../../astral-sh/ruff/pulls/9905.md) on 2024-02-09 03:11_

---

_Comment by @charliermarsh on 2024-02-09 03:14_

Put up a PR to fix here: https://github.com/astral-sh/ruff/pull/9905

---

_Closed by @charliermarsh on 2024-02-09 14:02_

---
