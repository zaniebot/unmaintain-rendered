---
number: 7948
title: Insert newline after docstring even when there are comments
type: issue
state: closed
author: konstin
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-10-13T15:50:12Z
updated_at: 2023-10-30T13:18:56Z
url: https://github.com/astral-sh/ruff/issues/7948
synced_at: 2026-01-07T13:12:15-06:00
---

# Insert newline after docstring even when there are comments

---

_Issue opened by @konstin on 2023-10-13 15:50_

Ours:
```pyhton
class ModuleBrowser:
    """Browse module classes and functions in IDLE."""
    # This class is also the base class for pathbrowser.PathBrowser.

    def __init__(self, master, path, *, _htest=False, _utest=False):
        pass
```
Black:
```python
class ModuleBrowser:
    """Browse module classes and functions in IDLE."""

    # This class is also the base class for pathbrowser.PathBrowser.

    def __init__(self, master, path, *, _htest=False, _utest=False):
        pass
```
We should also insert the empty line after the docstring even if there is comment

---

_Label `bug` added by @konstin on 2023-10-13 15:50_

---

_Label `formatter` added by @konstin on 2023-10-13 15:50_

---

_Comment by @charliermarsh on 2023-10-13 15:57_

This _might_ be intentional (though whether it’s good is debatable) — I believe we intentionally don’t insert a newline like that if it immediately follows an import, unlike Black.

---

_Comment by @charliermarsh on 2023-10-13 15:58_

What if there’s no empty line between the comment and the function definition?

---

_Comment by @konstin on 2023-10-13 15:59_

> What if there’s no empty line between the comment and the function definition?

In that case both we do the same thing as black, one empty line between docstring and the own line comment

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-10-16 07:44_

---

_Assigned to @konstin by @konstin on 2023-10-20 10:44_

---

_Referenced in [astral-sh/ruff#8216](../../astral-sh/ruff/pulls/8216.md) on 2023-10-25 13:44_

---

_Closed by @konstin on 2023-10-30 13:18_

---
