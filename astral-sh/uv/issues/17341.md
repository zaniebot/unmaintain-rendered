```yaml
number: 17341
title: "`pyside6-deploy` fails with `Failed to canonicalize script path`"
type: issue
state: open
author: docentYT
labels:
  - needs-mre
assignees: []
created_at: 2026-01-06T22:53:40Z
updated_at: 2026-01-07T20:28:34Z
url: https://github.com/astral-sh/uv/issues/17341
synced_at: 2026-01-10T03:11:36Z
```

# `pyside6-deploy` fails with `Failed to canonicalize script path`

---

_Issue opened by @docentYT on 2026-01-06 22:53_

### Summary

I am trying to deploy my PyQt6 app with the [`pyside6-deploy`](https://doc.qt.io/qtforpython-6/deployment/deployment-pyside6-deploy.html#pyside6-deploy) command.  I am getting the error:
`Failed to canonicalize script path` regardless of the path to the `main.py` I pass as the argument.

The command works when using a normal venv created by a 'classic' Python distribution.

### Platform

Windows 11 x64

### Version

uv 0.9.18 (0cee76417 2025-12-16)

### Python version

Python 3.13.5

---

_Label `bug` added by @docentYT on 2026-01-06 22:53_

---

_Comment by @konstin on 2026-01-07 08:50_

Can you share commands to reproduce the problem, and the full logs?

---

_Label `bug` removed by @konstin on 2026-01-07 08:51_

---

_Label `needs-mre` added by @konstin on 2026-01-07 08:51_

---

_Comment by @docentYT on 2026-01-07 20:28_

I wanted to share commands with you, but the error changed to: `No module named pip,` although I had tested this error before reporting it.

```powershell
uv venv
.\.venv\Scripts\Activate
uv pip install pyside6
```
Create `main.py`:
```py
from PySide6.QtWidgets import QApplication, QWidget

import sys

if __name__ == "__main__":
    app = QApplication(sys.argv)
    win = QWidget()
    win.show()
    sys.exit(app.exec())
```

```powershell
pyside6-deploy .\main.py
```

---
