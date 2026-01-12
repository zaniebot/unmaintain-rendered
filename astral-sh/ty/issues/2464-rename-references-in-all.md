```yaml
number: 2464
title: "Rename references in `__all__`"
type: issue
state: open
author: niclasvaneyk-teaminternet
labels:
  - server
assignees: []
created_at: 2026-01-12T13:16:16Z
updated_at: 2026-01-12T13:50:46Z
url: https://github.com/astral-sh/ty/issues/2464
synced_at: 2026-01-12T15:54:26Z
```

# Rename references in `__all__`

---

_@niclasvaneyk-teaminternet_

### Summary

Given the following code

```python
def my_function() -> str:
    return "Hello World"


__all__ = ["my_function"]
```

I want to rename `my_function` to `a_function` via `ty`s language server.

## Expected Result

```python
def a_function() -> str:
    return "Hello World"


__all__ = ["a_function"]
```

## Actual Result

```python
def a_function() -> str:
    return "Hello World"


__all__ = ["my_function"]  # <-- Still "my_" instead of "a_"
```



### Version

ty 0.0.11 (830cb9cc6 2026-01-09)

---

_Label `server` added by @AlexWaygood on 2026-01-12 13:16_

---

_Renamed from "textDocument/rename misses __all__ reference" to "Rename references in `__all__`" by @MichaReiser on 2026-01-12 13:50_

---

_Added to milestone `Stable` by @MichaReiser on 2026-01-12 13:50_

---
