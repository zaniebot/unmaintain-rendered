---
number: 1753
title: "Auto-fix for `SIM108` breaks `if sys.version_info ...` mypy convention"
type: issue
state: closed
author: edgarrmondragon
labels:
  - bug
  - fixes
assignees: []
created_at: 2023-01-09T21:04:53Z
updated_at: 2023-01-10T00:22:35Z
url: https://github.com/astral-sh/ruff/issues/1753
synced_at: 2026-01-10T01:22:39Z
---

# Auto-fix for `SIM108` breaks `if sys.version_info ...` mypy convention

---

_Issue opened by @edgarrmondragon on 2023-01-09 21:04_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->

```python
import random
import sys


def _get_random_bytes(n: int):
    return random.getrandbits(n * 8).to_bytes(n, "little")

if sys.version_info >= (3, 9):
    randbytes = random.randbytes
else:
    randbytes = _get_random_bytes
```

From [the Mypy docs](https://mypy.readthedocs.io/en/stable/common_issues.html#python-version-and-system-platform-checks):

<blockquote>

Mypy supports the ability to perform Python version checks and platform checks (e.g. Windows vs Posix), ignoring code paths that won’t be run on the targeted Python version or platform. This allows you to more effectively typecheck code that supports multiple versions of Python or multiple operating systems.

More specifically, mypy will understand the use of [sys.version_info](https://docs.python.org/3/library/sys.html#sys.version_info) and [sys.platform](https://docs.python.org/3/library/sys.html#sys.platform) checks within if/elif/else statements.

</blockquote>

so this works well with `mypy`:

```console
$ mypy ternary_sys.py --python-version 3.8
Success: no issues found in 1 source file
```

But the ternary operation created by the `SIM108` autofix breaks the above convention:

```console
$ ruff --version
ruff 0.0.216

$ ruff --select SIM108 ternary_sys.py
ternary_sys.py:8:1: SIM108 Use ternary operator `randbytes = random.randbytes if sys.version_info >= (3, 9) else _get_random_bytes` instead of if-else-block
Found 1 error(s).
1 potentially fixable with the --fix option.

$ ruff --select SIM108 ternary_sys.py --fix
Found 1 error(s) (1 fixed, 0 remaining).

$ mypy ternary_sys.py --python-version 3.8
ternary_sys.py:8: error: Module has no attribute "randbytes"  [attr-defined]
Found 1 error in 1 file (checked 1 source file)
```


---

_Comment by @edgarrmondragon on 2023-01-09 21:06_

Maybe the autofix should check if `sys.version_info` or `sys.platform` are referenced

---

_Referenced in [edgarrmondragon/citric#667](../../edgarrmondragon/citric/pulls/667.md) on 2023-01-09 21:07_

---

_Comment by @charliermarsh on 2023-01-09 22:01_

Sounds good, we can definitely special-case it.

---

_Label `bug` added by @charliermarsh on 2023-01-09 22:01_

---

_Label `autofix` added by @charliermarsh on 2023-01-09 22:01_

---

_Comment by @charliermarsh on 2023-01-09 22:02_

We already special-case “if name = main” so there’s precedent.

---

_Comment by @charliermarsh on 2023-01-09 22:29_

I can probably get to this tonight unless you want to @edgarrmondragon.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-09 23:17_

---

_Referenced in [astral-sh/ruff#1756](../../astral-sh/ruff/pulls/1756.md) on 2023-01-10 00:11_

---

_Closed by @charliermarsh on 2023-01-10 00:22_

---
