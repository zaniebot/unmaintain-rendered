```yaml
number: 19235
title: ANN201 NoReturn AutoFix overzealous on methods
type: issue
state: closed
author: Skylion007
labels: []
assignees: []
created_at: 2025-07-09T14:59:10Z
updated_at: 2025-07-09T15:30:20Z
url: https://github.com/astral-sh/ruff/issues/19235
synced_at: 2026-01-12T15:54:56Z
```

# ANN201 NoReturn AutoFix overzealous on methods

---

_@Skylion007_

### Summary

The RUFF ANN autofixes are a bit overzealous on adding NoReturn for methods, particularly for the cases of raise a NotImplementedError, a base class may implement a method with a different return type, and while yes NoReturn is valid for the base class, it may violate the substitution principal. This is really the only ANN autofix I have to be weary about, the rest are generally correct.

---

_Closed by @AlexWaygood on 2025-07-09 15:30_

---
