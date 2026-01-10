```yaml
number: 2830
title: "No error when checking for INP001, but `--add-noqa` adds directive"
type: issue
state: closed
author: hofrob
labels:
  - bug
assignees: []
created_at: 2023-02-12T22:28:11Z
updated_at: 2023-02-12T22:45:41Z
url: https://github.com/astral-sh/ruff/issues/2830
synced_at: 2026-01-10T11:09:45Z
```

# No error when checking for INP001, but `--add-noqa` adds directive

---

_Issue opened by @hofrob on 2023-02-12 22:28_

Python package:

```
foo/__init__.py
foo/bar.py  # empty
```

Contents of `__init__.py`:
```py
from foo import bar
```

Executing `ruff check -n --isolated --select INP001 foo/` does not show any error messages. But `ruff check -n --isolated --select INP001 --add-noqa foo/` adds a noqa directive.

Contents of `__init__.py` after `--add-noqa`:
```py
from foo import bar  # noqa: INP001
```

I don't think there's anything wrong with the code, so `--add-noqa` should not add any directives (in my opinion).

```
$ ruff --version
ruff 0.0.245
$ python --version
Python 3.11.2
```

:football: 

---

_Comment by @charliermarsh on 2023-02-12 22:31_

Oh very weird!

---

_Label `bug` added by @charliermarsh on 2023-02-12 22:31_

---

_Comment by @charliermarsh on 2023-02-12 22:39_

Ahhh right, there was a TODO to thread through the package here. Will fix...

---

_Closed by @charliermarsh on 2023-02-12 22:45_

---
