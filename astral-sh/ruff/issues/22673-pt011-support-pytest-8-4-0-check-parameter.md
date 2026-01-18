```yaml
number: 22673
title: "[PT011] Support pytest 8.4.0 `check` parameter"
type: issue
state: open
author: harupy
labels: []
assignees: []
created_at: 2026-01-18T10:24:03Z
updated_at: 2026-01-18T10:24:15Z
url: https://github.com/astral-sh/ruff/issues/22673
synced_at: 2026-01-18T11:18:14Z
```

# [PT011] Support pytest 8.4.0 `check` parameter

---

_@harupy_

### Summary

[PT011] Support pytest 8.4.0 check parameter

Description

PT011 (`pytest-raises-too-broad`) should not trigger when check is provided, as it narrows exception matching like match does.

```python
# Currently triggers PT011 (false positive)
with pytest.raises(OSError, check=lambda e: e.errno == errno.EACCES):
    ...
```

PT010 already handles this correctly.

References: https://github.com/pytest-dev/pytest/releases/tag/8.4.0


---

_Renamed from "[PT011] Support pytest 8.4.0 check parameter" to "[PT011] Support pytest 8.4.0 `check` parameter" by @harupy on 2026-01-18 10:24_

---
