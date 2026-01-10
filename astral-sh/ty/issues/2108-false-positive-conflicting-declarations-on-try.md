---
number: 2108
title: "False positive: `conflicting-declarations` on try/except import fallback pattern"
type: issue
state: open
author: The-Red-Dragons
labels:
  - bug
assignees: []
created_at: 2025-12-19T11:13:50Z
updated_at: 2025-12-23T05:15:29Z
url: https://github.com/astral-sh/ty/issues/2108
synced_at: 2026-01-10T01:52:52Z
---

# False positive: `conflicting-declarations` on try/except import fallback pattern

---

_Issue opened by @The-Red-Dragons on 2025-12-19 11:13_

### Summary

## Summary

ty reports `conflicting-declarations` when using the common Python pattern for optional imports with a fallback to `None`.

## Environment

- ty version: 0.0.4
- Python version: 3.14
- OS: Ubuntu 24.04

## Reproduction

```python
from mymodule import MyClass as MyClassType

# Type declaration for the variable
translator: MyClassType | None

try:
    from mymodule import translator
except (ImportError, ModuleNotFoundError, AttributeError):
    translator = None
```

## Expected Behavior

No error - this is a common pattern for optional dependencies where:
1. The type is declared as `T | None`
2. We try to import the real value
3. On failure, we fall back to `None`

## Actual Behavior

```
error[conflicting-declarations]: Conflicting declared types for `translator`: `MyClass | None` and `MyClass`
```

## Real-world Use Case

This pattern is widely used for optional features:
```python
# Optional translator support
translator: Translator | None

try:
    from utils.localization import translator
except ImportError:
    translator = None  # Feature disabled if not installed

# Later in code
if translator is not None:
    message = translator.t("key")
```

## Workaround

Using `conflicting-declarations = "warn"` in configuration.

### Version

0.0.4

---

_Comment by @carljm on 2025-12-22 23:51_

Here's a version that ty is currently happy with:

```py
try:
    from os import environ
except (ImportError, ModuleNotFoundError, AttributeError):
    environ: None = None
```

But I'm not claiming this version is better, and other type-checkers won't like it. I think we should be happy with the OP version.

One way to get there is #1836 

Another way could be to just silence `conflicting-declarations` (it's not necessary for soundness) and instead union the declarations. (Given that this is a reasonable option, it's also a reasonable workaround to just use `conflicting-declarations = "ignore"` in your `tool.ty.rules` config.)

---

_Added to milestone `Stable` by @carljm on 2025-12-22 23:51_

---

_Label `bug` added by @dhruvmanila on 2025-12-23 05:15_

---
