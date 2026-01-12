```yaml
number: 7172
title: Rule D202 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-05T17:45:55Z
updated_at: 2025-07-14T17:31:37Z
url: https://github.com/astral-sh/ruff/issues/7172
synced_at: 2026-01-12T15:54:46Z
```

# Rule D202 cause autofix error

---

_@qarmin_


Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select D202 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
def check_assert_called_once_with(logical_line, filename):
    """Try to detect unintended calls of nonexistent mock methods like:
    """\

    if 'ovn_octavia_provider/tests/' in filename:
            return
```

error
```
/home/rafal/test/tmp_folder/154_IDX_0_RAND_100719810512166459037745.py:2:5: D202 No blank lines allowed after function docstring (found 1)
Found 1 error.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/154_IDX_0_RAND_100719810512166459037745.py`, the rule codes D202, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/12526619/python_compressed.zip)


---

_Comment by @dhruvmanila on 2023-09-06 01:50_

It's the line continuation character which is creating this issue. There are 2 visible newlines but the first one is escaped so there's actually only 1 which gets removed. So, the token stream would produce a string and then directly the `if` token which is an invalid syntax.

---

_Label `bug` added by @dhruvmanila on 2023-09-06 01:50_

---

_Label `fuzzer` added by @dhruvmanila on 2023-09-06 01:50_

---

_Closed by @ntBre on 2025-07-14 17:31_

---
