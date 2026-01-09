---
number: 13068
title: "False Positive: F821(Undefined name) Error with `Literal`"
type: issue
state: closed
author: hawk-tomy
labels:
  - question
assignees: []
created_at: 2024-08-23T06:06:40Z
updated_at: 2024-08-23T06:18:58Z
url: https://github.com/astral-sh/ruff/issues/13068
synced_at: 2026-01-07T13:12:15-06:00
---

# False Positive: F821(Undefined name) Error with `Literal`

---

_Issue opened by @hawk-tomy on 2024-08-23 06:06_

```py
from typings import Literal

type AllowedStringType = Literal['Allowed1','Allowed2']
```

F821 error on `Allowed1` and `Allowed2`.
[playground](https://play.ruff.rs/468a5656-f6f9-42a3-9f46-bb707aeb83b4)

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

---

_Renamed from "False Positive: F821 Error with `Literal`" to "False Positive: F821(Undefined name) Error with `Literal`" by @hawk-tomy on 2024-08-23 06:09_

---

_Comment by @MichaReiser on 2024-08-23 06:13_

Can you try changing the import from `typing`**s** to `typing`? 

https://play.ruff.rs/0fc19fc9-330b-42a3-968e-b819bc2bb153

---

_Label `question` added by @MichaReiser on 2024-08-23 06:13_

---

_Comment by @hawk-tomy on 2024-08-23 06:18_

It was my mistake. Thank you.

---

_Closed by @hawk-tomy on 2024-08-23 06:18_

---
