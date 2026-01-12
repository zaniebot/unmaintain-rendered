```yaml
number: 1997
title: "Improve diagnostics for bad `__getitem__` calls"
type: issue
state: open
author: AlexWaygood
labels:
  - diagnostics
assignees: []
created_at: 2025-12-17T10:35:56Z
updated_at: 2025-12-17T10:35:56Z
url: https://github.com/astral-sh/ty/issues/1997
synced_at: 2026-01-12T15:54:26Z
```

# Improve diagnostics for bad `__getitem__` calls

---

_@AlexWaygood_

### Summary

This (seen on https://github.com/astral-sh/ruff/pull/22019#issuecomment-3664706394) is horrible UX for a diagnostic message:

```
+ pandera/engines/pandas_engine.py:1390:25: error[invalid-argument-type] Method `__getitem__` of type `Overload[(idx: int | signedinteger[_64Bit] | integer[Any] | signedinteger[_8Bit]) -> Unknown, (idx: Index[Any] | Series[Any] | slice[Any, Any, Any] | ndarray[tuple[Any, ...], dtype[integer[Any]]]) -> Series[Unknown], (idx: str | bytes | date | ... omitted 10 union elements) -> Unknown, (idx: Series[builtins.bool] | ndarray[tuple[Any, ...], dtype[numpy.bool[builtins.bool]]] | list[builtins.bool] | ... omitted 7 union elements) -> Series[Unknown]]` cannot be called with key of type `Series[bool] | DataFrame` on object of type `Series[Unknown]`
```

### Version

_No response_

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-17 10:35_

---

_Label `diagnostics` added by @AlexWaygood on 2025-12-17 10:35_

---
