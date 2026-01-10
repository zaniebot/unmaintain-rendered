---
number: 7087
title: Rules C404 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - help wanted
  - fuzzer
assignees: []
created_at: 2023-09-03T15:59:42Z
updated_at: 2023-09-06T12:05:41Z
url: https://github.com/astral-sh/ruff/issues/7087
synced_at: 2026-01-10T01:22:46Z
---

# Rules C404 cause autofix error

---

_Issue opened by @qarmin on 2023-09-03 15:59_


Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select C404 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
def save_data(data, company, model, unique, name_cleaner, value_cleaner, exclude):
            unique_instance = create_instance(row, model, name_cleaner=name_cleaner, value_cleaner=value_cleaner,
                                              unique=unique, company=company, exclude=exclude)
            try:
                unique_instance.save()
            except Exception as e:
                continue
            saved.append(dict([(k, v)for k,v in list(unique_instance.__dict__.items()) if k in [f.name for f in unique_instance._meta.fields]]))
```

error
```
/home/rafal/test/tmp_folder/785771.py:8:26: C404 Unnecessary `list` comprehension (rewrite as a `dict` comprehension)
Found 1 error.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/785771.py`, the rule codes C404, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!


```
        
[python_compressed.zip](https://github.com/astral-sh/ruff/files/12506512/python_compressed.zip)


---

_Renamed from "Rules C404 cause cause autofix error" to "Rules C404 cause autofix error" by @qarmin on 2023-09-03 16:00_

---

_Comment by @charliermarsh on 2023-09-03 16:37_

Basically the same as https://github.com/astral-sh/ruff/issues/7086 -- we need space between the value and the `for`.

---

_Label `bug` added by @charliermarsh on 2023-09-03 16:37_

---

_Label `fuzzer` added by @charliermarsh on 2023-09-03 16:37_

---

_Label `help wanted` added by @MichaReiser on 2023-09-04 06:32_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-05 17:26_

---

_Referenced in [astral-sh/ruff#7185](../../astral-sh/ruff/pulls/7185.md) on 2023-09-06 11:49_

---

_Closed by @charliermarsh on 2023-09-06 12:05_

---
