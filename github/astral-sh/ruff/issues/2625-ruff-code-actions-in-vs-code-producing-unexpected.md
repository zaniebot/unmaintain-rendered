---
number: 2625
title: Ruff code actions in VS Code producing unexpected output
type: issue
state: closed
author: JRyanHall94
labels:
  - bug
assignees: []
created_at: 2023-02-07T15:25:24Z
updated_at: 2023-02-09T16:57:32Z
url: https://github.com/astral-sh/ruff/issues/2625
synced_at: 2026-01-07T13:12:14-06:00
---

# Ruff code actions in VS Code producing unexpected output

---

_Issue opened by @JRyanHall94 on 2023-02-07 15:25_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
My issue is that when I save this file with these settings, I get unexpected behavior. Perhaps it's just the way that I have everything ordered in settings, but I tried adjusting things and couldn't figure out how to resolve this issue. Thanks in advance!

I am using black and ruff installed locally and I am formatting a file within an active venv in VS Code. My settings and the code are posted below, along with the unexpected output:

```
from django.contrib.auth.models import AbstractUser
from django.db import models

class CustomUser(AbstractUser):
    pass

```
becomes
```
from django.contrib.auth.models import AbstractUser


class CustomUser(AbstractUser):
    pass
    pass

```
----------
Settings in VS Code:
```
{
    "ruff.path": [
        "/Library/Frameworks/Python.framework/Versions/3.11/bin/ruff"
    ],
    "[python]": {
        "editor.codeActionsOnSave": {
            "source.fixAll": true,
            "source.organizeImports": true
        }
    },
    "editor.formatOnSave": true,
    "python.formatting.provider": "black",
    "python.formatting.blackPath": "/Library/Frameworks/Python.framework/Versions/3.11/bin/black"
}
```
current ruff version: ruff 0.0.243
current black version: black, 23.1.0 (compiled: yes)
current Python version: Python 3.11.1

note that if I remove the ruff portion of the settings, black formats the file as expected.

---

_Label `bug` added by @charliermarsh on 2023-02-07 15:40_

---

_Comment by @charliermarsh on 2023-02-07 15:40_

Thanks for filing! The problem is the double `pass` statement, right? The removal of `from django.db import models` is expected. Am I interpreting correctly?

---

_Comment by @JRyanHall94 on 2023-02-07 15:49_

Yes, absolutely 👍 

---

_Referenced in [astral-sh/ruff-vscode#128](../../astral-sh/ruff-vscode/issues/128.md) on 2023-02-08 15:41_

---

_Comment by @charliermarsh on 2023-02-09 16:57_

I'm just gonna close in favor of the corresponding issue in the `ruff-vscode` repo (linked above). Thank you for filing!

---

_Closed by @charliermarsh on 2023-02-09 16:57_

---
