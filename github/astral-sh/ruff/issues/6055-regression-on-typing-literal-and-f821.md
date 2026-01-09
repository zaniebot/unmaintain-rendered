---
number: 6055
title: Regression on typing.Literal and F821
type: issue
state: closed
author: hynek
labels:
  - bug
assignees: []
created_at: 2023-07-25T07:18:06Z
updated_at: 2023-07-25T15:02:04Z
url: https://github.com/astral-sh/ruff/issues/6055
synced_at: 2026-01-07T13:12:15-06:00
---

# Regression on typing.Literal and F821

---

_Issue opened by @hynek on 2023-07-25 07:18_

As of 0.0.279 and 0.0.280 this raises an F821:

```python
from __future__ import annotations

from typing import Literal


TokenType = Literal["reset"]
```

→

```console
$ ruff --version
ruff 0.0.280
$ ruff --isolated t.py
t.py:6:22: F821 Undefined name `reset`
Found 1 error.
$ pipx run --spec 'ruff==0.0.279' ruff --isolated --fix t.py
⚠️  ruff is already on your PATH and installed at /Users/hynek/Work/pw-reset/.venv/bin/ruff. Downloading and running anyway.
t.py:6:22: F821 Undefined name `reset`
Found 1 error.
$ pipx run --spec 'ruff==0.0.278' ruff --isolated --fix t.py
⚠️  ruff is already on your PATH and installed at /Users/hynek/Work/pw-reset/.venv/bin/ruff. Downloading and running anyway.
```

As you can see, the error goes away when going back to 0.0.278. Therefore the regression must come from 0.0.279.

Curiously, the error goes away, if I remove the future import.

---

_Comment by @charliermarsh on 2023-07-25 14:01_

I believe this is fixed by #6032 (and so will be out in the next release), sorry about that.

---

_Closed by @charliermarsh on 2023-07-25 14:01_

---

_Label `bug` added by @charliermarsh on 2023-07-25 14:01_

---

_Comment by @hynek on 2023-07-25 14:25_

huh how did I miss this while searching… sorry for the noise!

---

_Comment by @charliermarsh on 2023-07-25 15:02_

No prob! I felt like we had so much test coverage for F821, embarrassed we didn't have anything for this case 🙈 

---

_Referenced in [astral-sh/ruff#6014](../../astral-sh/ruff/issues/6014.md) on 2023-07-26 12:37_

---

_Referenced in [pylint-dev/pylint#8875](../../pylint-dev/pylint/pulls/8875.md) on 2023-07-26 13:02_

---
