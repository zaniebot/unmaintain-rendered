```yaml
number: 2017
title: "Add support for Sphinx-style \"attribute docstrings\" for on-hover functionality etc."
type: issue
state: closed
author: Julian-J-S
labels:
  - server
assignees: []
created_at: 2025-12-17T15:34:19Z
updated_at: 2025-12-18T12:18:22Z
url: https://github.com/astral-sh/ty/issues/2017
synced_at: 2026-01-12T15:54:26Z
```

# Add support for Sphinx-style "attribute docstrings" for on-hover functionality etc.

---

_@Julian-J-S_

Hover documentation is very minimal and missing the actual docstrings on class attributes which are very useful!

Example:
```python
class Hello:
    world: str
    """My text."""

h = Hello()
_ = h.world
```

Hover over `world` inside the class or `h.world` 
pyright:

<img width="227" height="104" alt="Image" src="https://github.com/user-attachments/assets/7f8ad53b-9866-4672-8ffe-7af5c4a90d1c" />

ty:

<img width="56" height="54" alt="Image" src="https://github.com/user-attachments/assets/87578da2-e20e-4ee8-97bd-020c2eec8c22" />

---

_Label `server` added by @AlexWaygood on 2025-12-17 15:38_

---

_Renamed from "Hover documentation missing for class attributes" to "Add support for Sphinx-style "attribute docstrings" for on-hover functionality etc." by @AlexWaygood on 2025-12-17 15:38_

---

_Assigned to @Gankra by @Gankra on 2025-12-17 18:02_

---

_Closed by @Gankra on 2025-12-18 12:18_

---
