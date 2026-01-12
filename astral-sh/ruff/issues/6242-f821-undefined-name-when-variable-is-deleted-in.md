```yaml
number: 6242
title: "F821 \"Undefined name\" when variable is deleted in another branch"
type: issue
state: open
author: Louis-dM
labels:
  - bug
assignees: []
created_at: 2023-08-01T13:51:04Z
updated_at: 2024-09-09T13:55:42Z
url: https://github.com/astral-sh/ruff/issues/6242
synced_at: 2026-01-12T15:54:45Z
```

# F821 "Undefined name" when variable is deleted in another branch

---

_@Louis-dM_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Suppose I `del` a variable in an Exception branch:
```python
def my_fn(a: str) -> str:
    try:
        print(a)
    except Exception:
        del a
        raise
    else:
        return a
```
`a` must always exist in the `else` branch, but Ruff claims it's undefined:
```
~$ ruff foobar.py --isolated
foobar.py:8:16: F821 Undefined name `a`
Found 1 error.
```

(line 8 char 16 is the location of the `a` in the return statement)

This is on version 0.0.281:
```
~$ ruff --version
ruff 0.0.281
```

---

_Label `bug` added by @zanieb on 2023-08-01 13:59_

---

_Comment by @zanieb on 2023-08-01 13:59_

Thanks for the report!

---

_Assigned to @zanieb by @zanieb on 2023-08-01 13:59_

---

_Unassigned @zanieb by @charliermarsh on 2024-09-09 13:55_

---
