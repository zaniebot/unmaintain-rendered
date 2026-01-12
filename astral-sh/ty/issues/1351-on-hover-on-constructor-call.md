```yaml
number: 1351
title: On-hover on constructor call
type: issue
state: open
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-10-14T07:05:02Z
updated_at: 2025-11-14T08:35:40Z
url: https://github.com/astral-sh/ty/issues/1351
synced_at: 2026-01-12T15:54:25Z
```

# On-hover on constructor call

---

_@MichaReiser_



```py
class Bar2:
    def __init__(self, a :int):
        self.y = 2

b = Bar2(30)
```

Hovering `Bar(30)` should show the signature of the constructor rather than the class

Pylance:
<img width="412" height="186" alt="Image" src="https://github.com/user-attachments/assets/1d4422bf-92b0-4cf6-b7c9-33f921013214" />

---

_Label `server` added by @MichaReiser on 2025-10-14 07:05_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:35_

---
