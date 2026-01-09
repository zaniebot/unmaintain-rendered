---
number: 8702
title: F821 false positive for IPython display()
type: issue
state: closed
author: guyrosin
labels:
  - bug
assignees: []
created_at: 2023-11-15T18:52:58Z
updated_at: 2023-11-16T01:58:46Z
url: https://github.com/astral-sh/ruff/issues/8702
synced_at: 2026-01-07T13:12:15-06:00
---

# F821 false positive for IPython display()

---

_Issue opened by @guyrosin on 2023-11-15 18:52_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
F821 (undefined-name) gives an "Undefined name `display`" when IPython's [display()](https://ipython.readthedocs.io/en/stable/api/generated/IPython.display.html#IPython.display.display) method is used in jupyter notebooks, although this method is automatically made available without import ([reference](https://ipython.readthedocs.io/en/stable/api/generated/IPython.display.html#IPython.display.display)).

See related issue in https://github.com/microsoft/vscode-jupyter/issues/8539

<img width="325" alt="image" src="https://github.com/astral-sh/ruff/assets/1250162/cb91ec35-7c4d-46fc-8549-43014d6ddf28">

Ruff version: 0.1.5

---

_Label `bug` added by @charliermarsh on 2023-11-15 20:17_

---

_Comment by @charliermarsh on 2023-11-15 20:17_

Is there any consolidated documentation on the builtins that are exposed in Jupyter?

---

_Comment by @guyrosin on 2023-11-15 20:49_

Not that I know of... I think they are just display() and magic commands

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-15 21:13_

---

_Referenced in [astral-sh/ruff#8707](../../astral-sh/ruff/pulls/8707.md) on 2023-11-15 23:16_

---

_Closed by @charliermarsh on 2023-11-16 01:58_

---
