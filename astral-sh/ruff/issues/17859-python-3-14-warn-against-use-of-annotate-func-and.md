```yaml
number: 17859
title: "Python 3.14+: Warn against use of `__annotate_func__` and `__annotations_cache__`"
type: issue
state: open
author: JelleZijlstra
labels:
  - rule
  - python314
assignees: []
created_at: 2025-05-05T13:52:25Z
updated_at: 2025-05-05T14:13:17Z
url: https://github.com/astral-sh/ruff/issues/17859
synced_at: 2026-01-12T15:54:56Z
```

# Python 3.14+: Warn against use of `__annotate_func__` and `__annotations_cache__`

---

_@JelleZijlstra_

### Summary

The implementation of PEP 749 in Python 3.14 uses two private names that may appear in the class namespace, `__annotations_cache__` and `__annotate_func__`. Users should never use these names directly, either through attribute access (`cls.__annotations_cache__`) or through subscripting the `__dict__` (`cls.__dict__["__annotate_func__"]`). It would be nice for Ruff to warn about these.

---

_Label `rule` added by @AlexWaygood on 2025-05-05 14:13_

---

_Label `python314` added by @AlexWaygood on 2025-05-05 14:13_

---
