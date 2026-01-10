---
number: 15583
title: Incorrect Import Removal Causes Undefined Name Error in Ruff Auto-Fix
type: issue
state: closed
author: brupelo
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-01-19T17:07:41Z
updated_at: 2025-01-19T18:28:10Z
url: https://github.com/astral-sh/ruff/issues/15583
synced_at: 2026-01-10T01:22:56Z
---

# Incorrect Import Removal Causes Undefined Name Error in Ruff Auto-Fix

---

_Issue opened by @brupelo on 2025-01-19 17:07_

Testing last origin/main at f1af2a386388f5edee99e26a24864a59566283d2

**File: imports_invalid_removal\bad_boy.py**

    from PySide6.QtWidgets import (
        QSlider,
    )
    
    from PySide6.QtCore import (
        Qt,
        Qt,
    )
    
    
    class TimeSliderControl(QSlider):
        def __init__(self, parent=None):
            super().__init__(Qt.Horizontal, parent)
            self.setMinimum(0)
            self.setMaximum(100)
            self.setValue(0)

```
ruff check --fix bad_boy.py --output-format=concise && ruff format bad_boy.py
D:\sources\github\astral-sh\bugs-ruff\imports_invalid_removal\bad_boy.py:8:26: F821 Undefined name `Qt`
Found 3 errors (2 fixed, 1 remaining).
```

**File: imports_invalid_removal\good_boy.py**

    from PySide6.QtWidgets import (
        QSlider,
    )
    
    from PySide6.QtCore import (
        Qt,
    )
    
    
    class TimeSliderControl(QSlider):
        def __init__(self, parent=None):
            super().__init__(Qt.Horizontal, parent)
            self.setMinimum(0)
            self.setMaximum(100)
            self.setValue(0)

```
>> debug: cmd: ruff check --fix D:\sources\github\astral-sh\bugs-ruff\imports_invalid_removal\good_boy.py --output-format=concise && ruff format D:\sources\github\astral-sh\bugs-ruff\imports_invalid_removal\good_boy.py
>> debug: working_dir: D:\sources\github\astral-sh\bugs-ruff\imports_invalid_removal
Found 1 error (1 fixed, 0 remaining).
1 file reformatted
```

Leftover from related issue at https://github.com/astral-sh/ruff/issues/15182 

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-19 18:01_

---

_Label `bug` added by @charliermarsh on 2025-01-19 18:01_

---

_Label `fixes` added by @charliermarsh on 2025-01-19 18:01_

---

_Referenced in [astral-sh/ruff#15585](../../astral-sh/ruff/pulls/15585.md) on 2025-01-19 18:21_

---

_Closed by @charliermarsh on 2025-01-19 18:28_

---
