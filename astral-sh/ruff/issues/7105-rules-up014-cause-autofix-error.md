```yaml
number: 7105
title: Rules UP014 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-03T18:48:03Z
updated_at: 2023-09-03T21:41:49Z
url: https://github.com/astral-sh/ruff/issues/7105
synced_at: 2026-01-12T15:54:46Z
```

# Rules UP014 cause autofix error

---

_@qarmin_

Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select UP014 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
try:
    from typing import NamedTuple, Iterator, Iterable, Tuple, List, Union
except ImportError:  # pragma: no cover
    "Module 'typing' is optional for 2.7-compatible types in comments"
SchemaItem = NamedTuple('SchemaItem', fields=(
    ('property_key', str),
))
```

error
```
/home/rafal/test/tmp_folder/6062352.py:5:1: UP014 Convert `SchemaItem` from `NamedTuple` functional to class syntax
Found 1 error.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/6062352.py`, the rule codes UP014, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```



[python_compressed.zip](https://github.com/astral-sh/ruff/files/12506977/python_compressed.zip)


---

_Label `fuzzer` added by @zanieb on 2023-09-03 19:02_

---

_Label `bug` added by @charliermarsh on 2023-09-03 21:10_

---

_Comment by @charliermarsh on 2023-09-03 21:14_

I think this is a precedence error in the generator.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-03 21:16_

---

_Closed by @charliermarsh on 2023-09-03 21:41_

---
