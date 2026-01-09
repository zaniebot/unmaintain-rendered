---
number: 3627
title: "[Autofix error] Fails to fix file which formats output"
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2023-03-20T15:37:43Z
updated_at: 2023-03-21T00:17:47Z
url: https://github.com/astral-sh/ruff/issues/3627
synced_at: 2026-01-07T13:12:14-06:00
---

# [Autofix error] Fails to fix file which formats output

---

_Issue opened by @qarmin on 2023-03-20 15:37_

Ruff  a45753f462c7a53afd7f942ab3c6f9af3871bf1f

```
class AbstractFolderIO:
    def __repr__(self):
        return'<%s: %s>' % (self.__class__.__name__, self.path)
```
with 
```
ruff file.py --fix
```

```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `Desktop/RunEveryCommand/Ruff/Broken/23027file_io (3rd copy)0.py`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

Desktop/RunEveryCommand/Ruff/Broken/23027file_io (3rd copy)0.py:1:1: INP001 File `Desktop/RunEveryCommand/Ruff/Broken/23027file_io (3rd copy)0.py` is part of an implicit namespace package. Add an `__init__.py`.
Desktop/RunEveryCommand/Ruff/Broken/23027file_io (3rd copy)0.py:1:1: D100 Missing docstring in public module
Desktop/RunEveryCommand/Ruff/Broken/23027file_io (3rd copy)0.py:1:7: D101 Missing docstring in public class
Desktop/RunEveryCommand/Ruff/Broken/23027file_io (3rd copy)0.py:2:9: ANN204 Missing return type annotation for special method `__repr__`
Desktop/RunEveryCommand/Ruff/Broken/23027file_io (3rd copy)0.py:2:9: D105 Missing docstring in magic method
Desktop/RunEveryCommand/Ruff/Broken/23027file_io (3rd copy)0.py:2:18: ANN101 Missing type annotation for `self` in method
Desktop/RunEveryCommand/Ruff/Broken/23027file_io (3rd copy)0.py:3:15: Q000 Single quotes found but double quotes preferred
Desktop/RunEveryCommand/Ruff/Broken/23027file_io (3rd copy)0.py:3:15: UP031 Use format specifiers instead of percent format

```


---

_Comment by @qarmin on 2023-03-20 15:39_

Maybe same problem
```
def aix_platform(osname, version, release):
    return"{}-{}.{}".format(osname, version, release)
```

---

_Label `bug` added by @charliermarsh on 2023-03-20 16:06_

---

_Comment by @charliermarsh on 2023-03-20 21:04_

Yeah definitely. We need to add a space here.

---

_Referenced in [astral-sh/ruff#3636](../../astral-sh/ruff/pulls/3636.md) on 2023-03-20 22:15_

---

_Closed by @charliermarsh on 2023-03-21 00:17_

---
