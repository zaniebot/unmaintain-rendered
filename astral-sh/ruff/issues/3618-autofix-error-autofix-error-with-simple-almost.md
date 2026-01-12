```yaml
number: 3618
title: "[Autofix error] Autofix error with simple almost empty function"
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2023-03-20T08:01:06Z
updated_at: 2023-03-20T21:17:44Z
url: https://github.com/astral-sh/ruff/issues/3618
synced_at: 2026-01-12T15:54:43Z
```

# [Autofix error] Autofix error with simple almost empty function

---

_@qarmin_

Ruff  a45753f462c7a53afd7f942ab3c6f9af3871bf1f

```
class QCameraImageCapture(__PyQt5_QtCore.QObject, QMediaBindableInterface):
    def imageCodecDescription(self, p_str): # real signature unknown; restored from __doc__
        """ imageCodecDescription(self, str) -> str\ """
        return ""
```
with 
```
ruff file.py --fix
```

```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `Desktop/RunEveryCommand/Ruff/Broken/3740QCameraImageCapture53.py`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```


---

_Label `bug` added by @charliermarsh on 2023-03-20 15:11_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-20 21:12_

---

_Closed by @charliermarsh on 2023-03-20 21:17_

---
