---
number: 7102
title: Rules UP037 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-03T18:41:29Z
updated_at: 2025-01-10T07:46:03Z
url: https://github.com/astral-sh/ruff/issues/7102
synced_at: 2026-01-07T13:12:15-06:00
---

# Rules UP037 cause autofix error

---

_Issue opened by @qarmin on 2023-09-03 18:41_

Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select UP037 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
from __future__ import annotations
class IdentityLinearOperator(ConstantDiagLinearOperator):
    def _mul_matrix(
        other: Union[Float[torch.Tensor, "... #M #N"], Float[LinearOperator, "... #M #N"]],
    ) -> Float[LinearOperator, "... M N"]:
        return other
```

error
```
/home/rafal/test/tmp_folder/2589347.py:4:42: UP037 Remove quotes from type annotation
/home/rafal/test/tmp_folder/2589347.py:4:78: UP037 Remove quotes from type annotation
Found 2 errors.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/2589347.py`, the rule codes UP037, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!


```
        
[python_compressed.zip](https://github.com/astral-sh/ruff/files/12506893/python_compressed.zip)


---

_Label `fuzzer` added by @zanieb on 2023-09-03 19:02_

---

_Label `bug` added by @charliermarsh on 2023-09-03 21:01_

---

_Comment by @charliermarsh on 2023-09-03 22:07_

Hah that's a really tricky one. The annotation _is_ valid code (`... #M #N`), but it ends in a comment, so it's not valid to inline in that position 🤦 

---

_Referenced in [astral-sh/ruff#15337](../../astral-sh/ruff/pulls/15337.md) on 2025-01-08 07:30_

---

_Closed by @MichaReiser on 2025-01-10 07:46_

---
