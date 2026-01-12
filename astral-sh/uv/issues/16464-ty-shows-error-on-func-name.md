```yaml
number: 16464
title: "`ty` shows error on `func.__name__`"
type: issue
state: closed
author: mainaksamuel
labels:
  - bug
assignees: []
created_at: 2025-10-27T03:39:57Z
updated_at: 2025-10-27T19:07:55Z
url: https://github.com/astral-sh/uv/issues/16464
synced_at: 2026-01-12T16:02:32Z
```

# `ty` shows error on `func.__name__`

---

_@mainaksamuel_

### Summary

`ty` shows the error below when getting the string name of a function using `__name__` attribute. The function is annotated with `Callable[..., Any]` from `typing`

```log
Object of type `(...)->Any` has no attribute `__name__`
```

### Platform

linux

### Version

ty 0.0.1-alpha.24

### Python version

Python 3.13.5

---

_Label `bug` added by @mainaksamuel on 2025-10-27 03:39_

---

_Comment by @konstin on 2025-10-27 19:07_

This is the uv repository, not the ty repository. This is tracked in the ty repository as https://github.com/astral-sh/ty/issues/599.

---

_Closed by @konstin on 2025-10-27 19:07_

---
