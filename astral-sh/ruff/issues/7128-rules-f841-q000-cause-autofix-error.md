```yaml
number: 7128
title: Rules F841, Q000 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-04T11:47:44Z
updated_at: 2024-08-01T12:56:37Z
url: https://github.com/astral-sh/ruff/issues/7128
synced_at: 2026-01-12T15:54:46Z
```

# Rules F841, Q000 cause autofix error

---

_@qarmin_

Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select F841,Q000 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
class InvalidUsername(ValueError):
    msgmask = ""'Invalid username %(username)r'
```

error
```
/home/rafal/test/tmp_folder/3765700.py:2:17: Q000 Single quotes found but double quotes preferred
Found 1 error.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/3765700.py`, the rule codes Q000, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!
```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/12513448/python_compressed.zip)



---

_Label `bug` added by @MichaReiser on 2023-09-05 06:50_

---

_Label `fuzzer` added by @MichaReiser on 2023-09-05 06:50_

---

_Closed by @qarmin on 2024-08-01 12:56_

---
