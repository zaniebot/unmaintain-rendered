```yaml
number: 19437
title: "PTH201 should check all subclasses of `PurePath`"
type: issue
state: closed
author: dscorbett
labels:
  - rule
assignees: []
created_at: 2025-07-20T13:18:05Z
updated_at: 2025-08-01T02:18:08Z
url: https://github.com/astral-sh/ruff/issues/19437
synced_at: 2026-01-10T11:09:59Z
```

# PTH201 should check all subclasses of `PurePath`

---

_Issue opened by @dscorbett on 2025-07-20 13:18_

### Summary

[`path-constructor-current-directory` (PTH201)](https://docs.astral.sh/ruff/rules/path-constructor-current-directory/) checks `PurePath` and `Path`. It should also check the other standard-library subclasses of `PurePath`. [Example](https://play.ruff.rs/f24b41d2-8459-472e-987b-30df6137b4b6):
```python
from importlib.metadata import PackagePath
from pathlib import PosixPath, PurePosixPath, PureWindowsPath, WindowsPath
PackagePath(".")
PosixPath(".")
PurePosixPath(".")
PureWindowsPath(".")
WindowsPath(".")
```

### Version

ruff 0.12.4 (ee2759b36 2025-07-17)

---

_Label `rule` added by @ntBre on 2025-07-20 23:04_

---

_Closed by @ntBre on 2025-08-01 02:18_

---
