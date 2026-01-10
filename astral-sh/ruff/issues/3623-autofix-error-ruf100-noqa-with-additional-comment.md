---
number: 3623
title: "[Autofix error] RUF100 noqa with additional comment"
type: issue
state: closed
author: araffin
labels:
  - suppression
assignees: []
created_at: 2023-03-20T11:29:45Z
updated_at: 2023-03-20T15:09:28Z
url: https://github.com/astral-sh/ruff/issues/3623
synced_at: 2026-01-10T01:22:42Z
---

# [Autofix error] RUF100 noqa with additional comment

---

_Issue opened by @araffin on 2023-03-20 11:29_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Command: `ruff --select RUF test.py --fix`

Minimal code:
```python
from torch import nn as nn  # noqa: F401 pylint: disable=unused-import
```

Ruff version: 0.0.257

Side question: what is the recommended way to deal with multiple "ignore"? (for instance `noqa` for ruff and `type: ignore` when using `mypy`)

---

_Comment by @JonathanPlasse on 2023-03-20 12:52_

You should separate the multiple "ignore" by `#` (c.f. https://til.simonwillison.net/python/ignore-both-flake8-and-mypy).
Like this `# pylint: …` will not be removed.
```python
from torch import nn as nn  # noqa: F401 # pylint: disable=unused-import
```
A lot of tools need the `#` for it to work.
```python
# Ruff does not detect the noqa on the following line
id: int = ""  # pyright: ignore noqa: A001
# Pyright does not detect the ignore on the following line
id: int = ""  # noqa: A001 pyright: ignore
# These work as expected
id: int = ""  # pyright: ignore # noqa: A001
id: int = ""  # noqa: A001 # pyright: ignore
```

---

_Comment by @charliermarsh on 2023-03-20 15:09_

This is fixed on `main`, I think (#3589).

---

_Closed by @charliermarsh on 2023-03-20 15:09_

---

_Label `noqa` added by @charliermarsh on 2023-03-20 15:09_

---

_Comment by @charliermarsh on 2023-03-20 15:09_

(But yeah, using the extra hash will also fix it.)

---
