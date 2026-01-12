```yaml
number: 7158
title: Rules B016, RUF002 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-05T14:45:55Z
updated_at: 2023-09-05T17:22:20Z
url: https://github.com/astral-sh/ruff/issues/7158
synced_at: 2026-01-12T15:54:46Z
```

# Rules B016, RUF002 cause autofix error

---

_@qarmin_


Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select B016,RUF002 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
class StateCls:
		"""SCPI: INPut:FILTer:HPASs[:STATe] \×
		"""
```

error
```
/home/rafal/test/tmp_folder/57_IDX_0_RAND_82872687065196457347885.py:2:40: RUF002 Docstring contains ambiguous `×` (MULTIPLICATION SIGN). Did you mean `x` (LATIN SMALL LETTER X)?
Found 1 error.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/57_IDX_0_RAND_82872687065196457347885.py`, the rule codes RUF002, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```



---

_Comment by @zanieb on 2023-09-05 16:20_

`\×` is valid syntax but `\x` is not due to

```
SyntaxError: (unicode error) 'unicodeescape' codec can't decode bytes in position 1-2: truncated \xXX escape
```

---

_Label `bug` added by @zanieb on 2023-09-05 16:20_

---

_Label `fuzzer` added by @zanieb on 2023-09-05 16:20_

---

_Comment by @charliermarsh on 2023-09-05 16:22_

I think we should disable autofix for these. They're already marked as manual.

---

_Closed by @charliermarsh on 2023-09-05 17:22_

---
