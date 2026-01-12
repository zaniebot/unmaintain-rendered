```yaml
number: 9312
title: "`runtime-evaluated-decorators` not working for the annotations of decorated functions."
type: issue
state: closed
author: avandierast
labels:
  - bug
assignees: []
created_at: 2023-12-29T18:43:35Z
updated_at: 2024-02-23T01:55:40Z
url: https://github.com/astral-sh/ruff/issues/9312
synced_at: 2026-01-12T15:54:49Z
```

# `runtime-evaluated-decorators` not working for the annotations of decorated functions.

---

_@avandierast_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Hello :)

First, thank you for the great work on ruff !

I'm trying to enforce TCH003 with `quote-annotations = true` and I need to exclude some decorated functions.
But it seems that `runtime-evaluated-decorators` does not work with functions but only with classes. Or I'm doing something wrong...

A minimal reproducing code snippet:
```python
# filename: TCH003/__init__.py

from pathlib import Path
from uuid import UUID

def runtime_evaluated(obj):
    return obj

@runtime_evaluated
class Toto:
    id: UUID

@runtime_evaluated
def toto(
    id: UUID,
    path: Path,
) -> None:
    pass
```

The corresponding ruff config:
```toml
select = ["TCH003"]

[lint.flake8-type-checking]
quote-annotations = true
runtime-evaluated-decorators = ["TCH003.__init__.runtime_evaluated"]
```

Now, ruff outputs:
```bash
$ ruff --version
ruff 0.1.9
$ ruff --config .ruff.toml 
TCH003/__init__.py:3:21: TCH003 Move standard library import `pathlib.Path` into a type-checking block
Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

The setting `runtime-evaluated-decorators` is working for classes since only `pathlib.Path` is subject to TCH003. But it does not work for the function.

The end goal of this is to make TCH00X rules work with FastAPI routers .
I found this issue but I failed to make it work : https://github.com/astral-sh/ruff/issues/6137.

Thank you for your help :)





---

_Comment by @divaltor on 2023-12-29 19:17_

I've messed up with same issue around `@validate_call` from `pydantic` module

It would be nice if Ruff would take such decorators into account while discovering which types should be quoted

```python
from app import User

from pydantic import validate_call

@validate_call(config={'arbitrary_types_allowed': True})
def test(user: User):
    ...
```

Converts to

```python
from typing import TYPE_CHECKING

from pydantic import validate_call

if TYPE_CHECKING:
    from app import User

@validate_call(config={'arbitrary_types_allowed': True})
def test(user: 'User'):
     ...
```

Which cause a `NameError` from `pydantic`

---

_Comment by @charliermarsh on 2023-12-29 20:11_

It looks like we don't apply these to functions at all right now (but we probably should?).

---

_Label `bug` added by @charliermarsh on 2023-12-29 20:11_

---

_Comment by @zanieb on 2023-12-29 20:13_

Hm the documentation for `runtime-evaluated-decorators` says

> Exempt **classes** decorated with any of the enumerated decorators from needing to be moved into type-checking blocks.

We can definitely consider expanding the scope of the setting, I do not know why it was originally limited.

It's only used at https://github.com/astral-sh/ruff/blob/2895e7d1266c661b36bbeeea31c293bdf5fdf0e7/crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs#L74-L92 

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-29 22:23_

---

_Comment by @charliermarsh on 2023-12-29 22:33_

PR is up :)

---

_Closed by @charliermarsh on 2023-12-30 02:14_

---

_Comment by @DetachHead on 2024-02-23 01:55_

this seems to still be broken in ruff 0.2.2. i've raised a new issue #10089

---
