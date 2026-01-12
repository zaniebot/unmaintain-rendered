```yaml
number: 8141
title: "Should `RUF013` cover type hints in `AnnAssign`?"
type: issue
state: closed
author: harupy
labels: []
assignees: []
created_at: 2023-10-23T11:56:26Z
updated_at: 2023-10-23T15:09:07Z
url: https://github.com/astral-sh/ruff/issues/8141
synced_at: 2026-01-12T15:54:47Z
```

# Should `RUF013` cover type hints in `AnnAssign`?

---

_@harupy_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

```python
def f(a: int = None):
       # ^^^ RUF013
    pass


# Good (but should be bad?)
a: int = None


class Foo:
    a: int = None
```

https://play.ruff.rs/96716587-c53e-4175-a1fc-386b0a25966d

---

_Comment by @tmke8 on 2023-10-23 14:28_

I think implicit-optional only ever applied to function definitions, so while the other two assignments clearly violate type rules, they were never accepted by type checkers, so I would say ruff doesn't need a rule for them.

---

_Closed by @harupy on 2023-10-23 14:55_

---

_Comment by @harupy on 2023-10-23 15:09_

Agreed :)

---
