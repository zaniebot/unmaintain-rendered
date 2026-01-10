```yaml
number: 3496
title: Trailing added list not autofixed by RUF005
type: issue
state: closed
author: Pierre-Sassoulas
labels:
  - fixes
assignees: []
created_at: 2023-03-13T22:46:59Z
updated_at: 2023-05-24T01:26:35Z
url: https://github.com/astral-sh/ruff/issues/3496
synced_at: 2026-01-10T11:09:46Z
```

# Trailing added list not autofixed by RUF005

---

_Issue opened by @Pierre-Sassoulas on 2023-03-13 22:46_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
In the following snippet called a.py;
```python
import sys
from pathlib import Path
import pytest


@pytest.mark.parametrize(
    "args",
    [
        ["--disable=import-error,unused-import"],
    ],
)
def test_a(self, tmp_path: Path, args: list[str]) -> None:
    for path in ("astroid.py", "hmac.py"):
        pylint_call = [sys.executable, "-m", "pylint"] + args + [path]
        print(pylint_call)
```
The fix for RUF05 when launched with ``ruff a.py --fix --select="RUF"`` is:
```python
pylint_call = [sys.executable, "-m", "pylint", *args] + [path]
```
Unless I'm mistaken it should be 
```python
pylint_call = [sys.executable, "-m", "pylint", *args, path]
```

---

_Label `autofix` added by @charliermarsh on 2023-03-13 22:54_

---

_Renamed from "Trailing list list not autofixed by RUF005" to "Trailing added list not autofixed by RUF005" by @Pierre-Sassoulas on 2023-03-14 12:20_

---

_Comment by @hoel-bagard on 2023-05-20 18:31_

Hi, I'm looking into it.

---

_Closed by @charliermarsh on 2023-05-24 01:26_

---
