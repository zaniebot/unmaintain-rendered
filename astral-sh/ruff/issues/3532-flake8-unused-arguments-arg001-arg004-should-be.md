```yaml
number: 3532
title: "flake8-unused-arguments: ARG001-ARG004 should be disabled for `.pyi` files"
type: issue
state: closed
author: XuehaiPan
labels:
  - bug
assignees: []
created_at: 2023-03-15T02:52:42Z
updated_at: 2023-03-15T03:17:21Z
url: https://github.com/astral-sh/ruff/issues/3532
synced_at: 2026-01-12T15:54:43Z
```

# flake8-unused-arguments: ARG001-ARG004 should be disabled for `.pyi` files

---

_@XuehaiPan_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Thanks for the amazing tool. I'm gradually enabling as many as possible rules that `ruff` provides in my project. I'm running `ruff check --select=ALL .` and selecting reasonable rule code in my `pyproject.toml`.

I found `ruff check .` will glob all `.py` and `.pyi` in the directory. The `ARG001-ARG004` creates too much noise here for unused arguments in `.pyi` stub files. The function definitions in stub files are intentionally not to have implementation. So the arguments are never used.

```python
# test.pyi

def add(x: float, y: float) -> float: ...
```

```console
$ ruff --version
ruff 0.0.255

$ ruff check --select=ARG test.pyi
test.pyi:3:9: ARG001 Unused function argument: `x`
  |
3 | def add(x: float, y: float) -> float: ...
  |         ^^^^^^^^ ARG001
  |

test.pyi:3:19: ARG001 Unused function argument: `y`
  |
3 | def add(x: float, y: float) -> float: ...
  |                   ^^^^^^^^ ARG001
  |

Found 2 errors.
```


---

_Comment by @charliermarsh on 2023-03-15 03:09_

Oh yeah, definitely not.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-15 03:09_

---

_Label `bug` added by @charliermarsh on 2023-03-15 03:10_

---

_Closed by @charliermarsh on 2023-03-15 03:17_

---
