```yaml
number: 18375
title: "UP036 doesn't work for ternary expressions"
type: issue
state: open
author: Andrej730
labels:
  - rule
assignees: []
created_at: 2025-05-29T16:26:36Z
updated_at: 2025-05-29T16:47:51Z
url: https://github.com/astral-sh/ruff/issues/18375
synced_at: 2026-01-12T15:54:56Z
```

# UP036 doesn't work for ternary expressions

---

_@Andrej730_

### Summary

See the snippet below, can be checked with `ruff check test.py --select UP036 --target-version py39`.

```python
import sys

# In theory it's UP036, but undetected.
x = "a" if sys.version_info < (3, 8) else "b"

# UP036 Version block is outdated for minimum Python version
if sys.version_info < (3, 8):
    y = "a"
else:
    y = "b"
```

### Version

ruff 0.11.11 (0397682f1 2025-05-22)

---

_Comment by @MichaReiser on 2025-05-29 16:47_

Related to https://github.com/astral-sh/ruff/issues/16487

---

_Label `rule` added by @MichaReiser on 2025-05-29 16:47_

---
