---
number: 13269
title: "Add UP rule: rewrite TypeVar defaults for Generator / AsyncGenerator"
type: issue
state: closed
author: hugovk
labels:
  - rule
  - help wanted
  - accepted
assignees: []
created_at: 2024-09-06T12:49:44Z
updated_at: 2024-09-22T11:00:46Z
url: https://github.com/astral-sh/ruff/issues/13269
synced_at: 2026-01-07T13:12:15-06:00
---

# Add UP rule: rewrite TypeVar defaults for Generator / AsyncGenerator

---

_Issue opened by @hugovk on 2024-09-06 12:49_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

There's a new check in pyupgrade to rewrite using PEP 696 TypeVar defaults:

* https://github.com/asottile/pyupgrade#pep-696-typevar-defaults
* https://github.com/asottile/pyupgrade/pull/948

```diff
from collections.abc import Generator, AsyncGenerator

-def f() -> Generator[int, None, None]:
+def f() -> Generator[int]:
     yield 1
```
```diff
from collections.abc import Generator, AsyncGenerator

-async def f() -> AsyncGenerator[int, None]:
+async def f() -> AsyncGenerator[int]:
     yield 1
```

See the https://github.com/asottile/pyupgrade/pull/948/files for some variations.

---

_Label `rule` added by @AlexWaygood on 2024-09-07 12:35_

---

_Label `help wanted` added by @AlexWaygood on 2024-09-07 12:35_

---

_Label `accepted` added by @AlexWaygood on 2024-09-07 12:35_

---

_Comment by @iamlucasvieira on 2024-09-07 12:47_

May I work on this one? 

---

_Assigned to @iamlucasvieira by @MichaReiser on 2024-09-09 11:58_

---

_Comment by @iamlucasvieira on 2024-09-22 10:38_

I think we already have this rule implemented: UP043

---

_Closed by @MichaReiser on 2024-09-22 11:00_

---
