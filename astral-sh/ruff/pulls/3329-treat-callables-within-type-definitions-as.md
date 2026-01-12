```yaml
number: 3329
title: Treat callables within type definitions as default-non-types
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/call
created_at: 2023-03-03T22:54:55Z
updated_at: 2023-03-03T23:07:32Z
url: https://github.com/astral-sh/ruff/pull/3329
synced_at: 2026-01-12T04:39:44Z
```

# Treat callables within type definitions as default-non-types

---

_Pull request opened by @charliermarsh on 2023-03-03 22:54_

We need to avoid treating `"FILE_PATH"`, below, as an undefined reference.

```py
class LIGHTSHOW_OT_letter_creation(bpy.types.Operator):
    bl_label = "Create Character"
    bl_idname = "lightshow.letter_creation"

    filepath: bpy.props.StringProperty(subtype="FILE_PATH")  # OK
```

At the same time, we _do_ want this to raise an undefined reference (if `os` is not imported):

```py
def f(x: Callable[[VarArg("os")], None]):  # F821
    pass
```

Closes #3307.

---

_Converted to draft by @charliermarsh on 2023-03-03 22:56_

---

_Marked ready for review by @charliermarsh on 2023-03-03 22:59_

---

_Merged by @charliermarsh on 2023-03-03 23:07_

---

_Closed by @charliermarsh on 2023-03-03 23:07_

---

_Branch deleted on 2023-03-03 23:07_

---
