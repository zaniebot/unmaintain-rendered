```yaml
number: 10235
title: The program cannot write to the clipboard in a PySide6 project.
type: issue
state: closed
author: vichbb
labels: []
assignees: []
created_at: 2024-12-30T10:02:52Z
updated_at: 2024-12-30T15:38:02Z
url: https://github.com/astral-sh/uv/issues/10235
synced_at: 2026-01-10T04:36:21Z
```

# The program cannot write to the clipboard in a PySide6 project.

---

_Issue opened by @vichbb on 2024-12-30 10:02_

```python
    def copy(self, text):
        clipboard = QApplication.clipboard()
        clipboard.setText(text)
```
I'm working on a Pyside6 project, and I found that on MacBook M4, this python can't write to the clipboard without any errors, but the python installed by brew can write to the clipboard without any errors，tried pbcopy also doesn't work。

---

_Comment by @charliermarsh on 2024-12-30 15:37_

My guess is that this is a python-build-standalone quirk and can be merged into https://github.com/astral-sh/python-build-standalone/issues/274.

---

_Closed by @charliermarsh on 2024-12-30 15:38_

---
