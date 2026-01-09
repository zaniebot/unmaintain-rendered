---
number: 2408
title: "Query with TCH00[1-3] and `if False`"
type: issue
state: closed
author: AA-Turner
labels:
  - question
assignees: []
created_at: 2023-01-31T18:58:17Z
updated_at: 2023-02-02T17:36:00Z
url: https://github.com/astral-sh/ruff/issues/2408
synced_at: 2026-01-07T13:12:14-06:00
---

# Query with TCH00[1-3] and `if False`

---

_Issue opened by @AA-Turner on 2023-01-31 18:58_

Ruff version 0.0.238.

Sometimes in runtime code I use `if False` to avoid importing from `typing` if I would only be using `TYPE_CHECKING`. (All three forms pass MyPy.)

Ruff reports this as a TCH003 issue, I wonder if `if False` or `if 0` could be added to the detection heuristic currently used for `if TYPE_CHECKING`?

If you'd prefer not to, no worries -- this is easy to add a `# NoQA: TCH00X` comment to.

```powershell
(sphinx) PS I:\Development\sphinx> ruff --isolated --select TCH bug.py
bug.py:4:23: TCH003 Move standard library import `types.TracebackType` into a type-checking block
Found 1 error.
(sphinx) PS I:\Development\sphinx> type bug.py                                   
from __future__ import annotations

if False:
    from types import EllipsisType

spam: EllipsisType = ...
(sphinx) PS I:\Development\sphinx> ruff --isolated --select TCH bug.py
bug.py:4:23: TCH003 Move standard library import `types.EllipsisType` into a type-checking block
Found 1 error.
(sphinx) PS I:\Development\sphinx> ruff --version                     
ruff 0.0.238
(sphinx) PS I:\Development\sphinx> 
```

A

---

_Comment by @charliermarsh on 2023-01-31 19:10_

Woah, that works?

---

_Comment by @charliermarsh on 2023-01-31 19:10_

Let me think on it :joy:

---

_Comment by @charliermarsh on 2023-01-31 23:04_

I'm [reading](https://github.com/davidhalter/jedi/issues/1472) that `if False` was the convention in older Python versions (like 3.5.0).

I try not to support anything pre-3.7 but I guess in this case it costs me ~nothing to support it.

---

_Comment by @charliermarsh on 2023-01-31 23:05_

Can you verify for me that at least one other type checker (like Pyright or Pyre) respects this?

---

_Label `question` added by @charliermarsh on 2023-02-01 04:55_

---

_Comment by @AA-Turner on 2023-02-01 09:04_

Pyre passes on ``if False`` and ``if 0`` (through WSL, only ``if False`` shown here):

```bash
(venv) adam:/mnt/i/Development/Sphinx$ cat bug/bug.py
from __future__ import annotations

if False:
    from types import EllipsisType

spam: EllipsisType = ...
(venv) adam:/mnt/i/Development/Sphinx$ pyre --source-directory bug/ check
ƛ No type errors found
(venv311) adam@Gyrostats:/mnt/i/Development/Sphinx$
```

Pyright passes only with ``if 0``:

```powershell
PS I:\Development\sphinx> type bug.py
from __future__ import annotations

if 0:
    from types import EllipsisType

spam: EllipsisType = ...
PS I:\Development\sphinx> pyright bug.py
[...elided...]
pyright 1.1.292
0 errors, 0 warnings, 0 informations
Completed in 0.777sec
```

A


---

_Referenced in [astral-sh/ruff#2485](../../astral-sh/ruff/pulls/2485.md) on 2023-02-02 17:31_

---

_Closed by @charliermarsh on 2023-02-02 17:36_

---

_Referenced in [sphinx-doc/sphinx#11177](../../sphinx-doc/sphinx/pulls/11177.md) on 2023-02-04 09:53_

---
