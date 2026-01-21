```yaml
number: 22673
title: "[PT011] Support pytest 8.4.0 `check` parameter"
type: issue
state: closed
author: harupy
labels:
  - rule
assignees: []
created_at: 2026-01-18T10:24:03Z
updated_at: 2026-01-21T08:22:14Z
url: https://github.com/astral-sh/ruff/issues/22673
synced_at: 2026-01-21T09:02:51Z
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

_Comment by @ntBre on 2026-01-19 14:49_

Makes sense to me!

---

_Label `rule` added by @ntBre on 2026-01-19 14:50_

---

_Comment by @harupy on 2026-01-20 15:15_

@ntBre Thanks for the comment! Filed #22725

---

_Closed by @MichaReiser on 2026-01-21 08:22_

---
