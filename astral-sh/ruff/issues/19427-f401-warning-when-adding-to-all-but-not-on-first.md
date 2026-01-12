```yaml
number: 19427
title: F401 warning when adding to __all__, but not on first assignment
type: issue
state: open
author: mistydemeo
labels:
  - rule
assignees: []
created_at: 2025-07-18T21:59:28Z
updated_at: 2025-07-21T07:18:13Z
url: https://github.com/astral-sh/ruff/issues/19427
synced_at: 2026-01-12T15:54:56Z
```

# F401 warning when adding to __all__, but not on first assignment

---

_@mistydemeo_

### Summary

I've noticed that the F401 check permits reexports if those exports are listed in `__all__`, but still flags imports that are *added* to `__all__` later in the same file. For example:

```python
import os

__all__ = [
    "os"
]
```

Is fine, and doesn't flag `import os` as unnecessary. However, if you want to add to `__all__` later:

```python

import re

__all__.extend(["re"])
```

Is flagged as an error:

```
from bar import func
                ^^^^ F401

help: Remove unused import: `bar.func`
```

In some cases ruff will even outright tell you to "Add unused import `bar` to `__all__`", even if that's what the user was doing.

This primarily comes up for me in an application where certain features are gated by optional dependencies. So, for example, I might want to define `__all__` first with the reexports that are always available, and then add extra reexports from parts of the app that may or may not be available:

```python
if importlib.util.find_spec("yt_dlp"):
    from bar import func

    __all__.extend([
        "func",
    ])
```

### Version

ruff 0.12.4 (ee2759b36 2025-07-17)

---

_Label `rule` added by @MichaReiser on 2025-07-21 07:18_

---
