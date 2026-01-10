```yaml
number: 7101
title: Rules UP024 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-03T18:40:57Z
updated_at: 2023-09-05T17:37:11Z
url: https://github.com/astral-sh/ruff/issues/7101
synced_at: 2026-01-10T11:09:49Z
```

# Rules UP024 cause autofix error

---

_Issue opened by @qarmin on 2023-09-03 18:40_

Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select UP024 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
def get_owner_id_from_mac_address():
    try:
        mac_address = get_primary_mac_address()
    except(IOError, OSError) as ex:
        msg = 'Unable to query URL to get Owner ID: {u}\n{e}'.format(u=owner_id_url, e=ex)
```

error
```
/home/rafal/test/tmp_folder/2385644.py:4:11: UP024 Replace aliased errors with `OSError`
Found 1 error.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/2385644.py`, the rule codes UP024, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!


```
        

---

_Label `fuzzer` added by @zanieb on 2023-09-03 19:02_

---

_Label `bug` added by @charliermarsh on 2023-09-03 21:01_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-05 17:11_

---

_Closed by @charliermarsh on 2023-09-05 17:37_

---
