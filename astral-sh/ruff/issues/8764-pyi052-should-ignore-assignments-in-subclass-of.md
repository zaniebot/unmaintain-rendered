```yaml
number: 8764
title: PYI052 should ignore assignments in subclass of enum without enum members
type: issue
state: closed
author: henribru
labels:
  - bug
assignees: []
created_at: 2023-11-19T10:24:47Z
updated_at: 2023-11-19T15:26:11Z
url: https://github.com/astral-sh/ruff/issues/8764
synced_at: 2026-01-10T11:09:51Z
```

# PYI052 should ignore assignments in subclass of enum without enum members

---

_Issue opened by @henribru on 2023-11-19 10:24_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

PYI052 correctly accepts the following code:
```python
from enum import Enum

class Baz(Enum):
    FOO = 0
    BAR = 1
```

However, it flags the assignments to `FOO` and `BAR` in this code:
```python
from enum import Enum

class Foo(Enum):
    ...

class Bar(Foo):
    FOO = 0
    BAR = 1
```

Inheriting from enums without enum members works at runtime and is understood by typecheckers, so I think these two examples should be treated the same. https://mypy-play.net/?mypy=latest&python=3.11&gist=cdb6a82662cbb77416bb64692606cb83

Reproduced with v0.1.6 running `ruff foo.pyi --select PYI052` without any settings in `pyproject.toml`.

---

_Renamed from "PYI052 should ignore assignments in subclass of empty enum" to "PYI052 should ignore assignments in subclass of enum without enum members" by @henribru on 2023-11-19 10:25_

---

_Label `bug` added by @charliermarsh on 2023-11-19 13:58_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-19 14:03_

---

_Closed by @charliermarsh on 2023-11-19 14:32_

---

_Comment by @henribru on 2023-11-19 15:26_

Thanks for the prompt fix as always, you're awesome!

---
