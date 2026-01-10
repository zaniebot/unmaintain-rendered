---
number: 7197
title: Rule FLY002 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-06T15:30:06Z
updated_at: 2023-09-07T14:32:45Z
url: https://github.com/astral-sh/ruff/issues/7197
synced_at: 2026-01-10T01:22:46Z
---

# Rule FLY002 cause autofix error

---

_Issue opened by @qarmin on 2023-09-06 15:30_

Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select FLY002 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
def creae_file_publc_url(url, filename):
    return''.join([url, flename])
```

error
```
/home/rafal/test/tmp_folder/882_IDX_0_RAND_114325313818141405238226.py:2:11: FLY002 Consider `f'{url}{flename}'` instead of string join
Found 1 error.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/882_IDX_0_RAND_114325313818141405238226.py`, the rule codes FLY002, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/12539665/python_compressed.zip)


---

_Label `bug` added by @charliermarsh on 2023-09-07 14:08_

---

_Label `fuzzer` added by @charliermarsh on 2023-09-07 14:08_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-07 14:22_

---

_Referenced in [astral-sh/ruff#7222](../../astral-sh/ruff/pulls/7222.md) on 2023-09-07 14:22_

---

_Closed by @charliermarsh on 2023-09-07 14:32_

---
