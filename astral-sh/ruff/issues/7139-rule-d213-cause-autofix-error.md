```yaml
number: 7139
title: Rule D213 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-05T06:21:11Z
updated_at: 2023-09-06T08:51:52Z
url: https://github.com/astral-sh/ruff/issues/7139
synced_at: 2026-01-12T15:54:46Z
```

# Rule D213 cause autofix error

---

_@qarmin_

Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select D213 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
"\
 "#retuasrn "t" es/samba/dcerp#c/netlogon.cpyth
```

error
```
/home/rafal/test/tmp_folder/703936860PY_FILE_TEST_12151402529755704590.py:1:1: D213 Multi-line docstring summary should start at the second line
Found 1 error.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/703936860PY_FILE_TEST_12151402529755704590.py`, the rule codes D213, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/12519530/python_compressed.zip)


---

_Label `bug` added by @MichaReiser on 2023-09-05 07:00_

---

_Label `fuzzer` added by @MichaReiser on 2023-09-05 07:00_

---

_Assigned to @konstin by @konstin on 2023-09-05 19:30_

---

_Comment by @dhruvmanila on 2023-09-06 02:32_

#7140 looks similar and might be fixed by the linked PR.

---

_Closed by @konstin on 2023-09-06 08:51_

---
