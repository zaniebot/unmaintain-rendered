```yaml
number: 13930
title: False Positive F811 Redfinition on class member if global variable with same name is unused.
type: issue
state: closed
author: Daraan
labels:
  - question
assignees: []
created_at: 2024-10-25T16:33:33Z
updated_at: 2024-11-08T03:41:08Z
url: https://github.com/astral-sh/ruff/issues/13930
synced_at: 2026-01-10T11:09:55Z
```

# False Positive F811 Redfinition on class member if global variable with same name is unused.

---

_Issue opened by @Daraan on 2024-10-25 16:33_

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

Version: 0.7.1

In the following code snippet the class member is marked as redefinition

https://play.ruff.rs/ce10f9c6-0895-4146-932c-89c3aea05159

```python
from queue import Empty

class Types:
    INVALID = 0
    UINT = 1
    HEX = 2
    Empty = 3  # <-- reported as redefinition

# q = Empty() <- this disables the error, if uncommented
```

---

_Renamed from "False Positive F811 Redfinition on class member" to "False Positive F811 Redfinition on class member if global variable with same name is unused." by @Daraan on 2024-10-25 16:34_

---

_Label `bug` added by @MichaReiser on 2024-10-26 08:09_

---

_Comment by @charliermarsh on 2024-10-28 01:05_

Pyflakes flags this, so I'm inclined to say it's working as expected.

---

_Comment by @MichaReiser on 2024-10-28 07:02_

Looking at this more closely, it does seem consistent with how we handle redefinitions in function bodies. 

https://play.ruff.rs/39ccfb6f-b0d6-4674-9fa1-6982797166fc

---

_Comment by @Daraan on 2024-10-28 08:11_

Thank you for looking at it. True, it is a redefinition I guess a warning is then helpfull.

I am then only confused about the variable usage of it that disables the error: https://play.ruff.rs/47aeefef-757c-47ec-afb0-7952478d0aeb

```python
q = Empty() # <- this disables the error, if uncommented
```

---

_Comment by @MichaReiser on 2024-10-30 07:53_

The reason that the usage disables the warning is that `F811` only warns about symbols that get redefined but are never used. Adding `q = Empty()` adds a use to `Empty`: It's still redefined, but no longer unused.

---

_Label `bug` removed by @MichaReiser on 2024-10-30 07:53_

---

_Label `question` added by @MichaReiser on 2024-10-30 07:53_

---

_Closed by @charliermarsh on 2024-11-08 03:41_

---
