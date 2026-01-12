```yaml
number: 328
title: "`if callable(func)` does not type narrow"
type: issue
state: closed
author: janosh
labels:
  - narrowing
assignees: []
created_at: 2025-05-12T13:01:22Z
updated_at: 2025-05-12T13:15:07Z
url: https://github.com/astral-sh/ty/issues/328
synced_at: 2026-01-12T15:54:22Z
```

# `if callable(func)` does not type narrow

---

_@janosh_

### Summary

https://play.ty.dev/b8a6e743-8f1c-4e92-ad1e-91d10abbdd70

```py
func = None

if callable(func):
    func() # Object of type `None` is not callable (call-non-callable) [Ln 4, Col 5]
```

[`mypy` works as expected](https://mypy-play.net/?mypy=latest&python=3.12&gist=e9f5ab7b4a38cd1316f6c62c7df17929)

### Version

ty 0.0.0-alpha.8 (0474b40e1 2025-05-09)

---

_Closed by @AlexWaygood on 2025-05-12 13:11_

---

_Comment by @AlexWaygood on 2025-05-12 13:12_

Thanks! This should naturally fall out as a result of implementing #117, since `callable()` uses `TypeIs` as its return annotation in typeshed's stubs 

---

_Label `narrowing` added by @AlexWaygood on 2025-05-12 13:15_

---
