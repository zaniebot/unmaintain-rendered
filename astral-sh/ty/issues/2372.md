```yaml
number: 2372
title: "isinstance(obj, dict) on object | None narrows to type where dict.get() fails with Expected Never"
type: issue
state: closed
author: yilei
labels: []
assignees: []
created_at: 2026-01-06T22:20:24Z
updated_at: 2026-01-06T23:14:29Z
url: https://github.com/astral-sh/ty/issues/2372
synced_at: 2026-01-10T01:56:41Z
```

# isinstance(obj, dict) on object | None narrows to type where dict.get() fails with Expected Never

---

_Issue opened by @yilei on 2026-01-06 22:20_

### Summary

When narrowing `object | None` to `dict` using `isinstance()`, ty reveals the type as `Top[dict[Unknown, Unknown]]`, but subsequent calls to `dict.get()` fail with `Expected Never, found Literal["key"]`. This makes common patterns for handling untyped JSON-like data unusable.

### Minimal Reproducer

```python
def get_value(body: object | None) -> str | None:
    if isinstance(body, dict):
        # ty reveals body as `Top[dict[Unknown, Unknown]]`
        # but dict.get() fails with "Expected Never, found Literal['key']"
        value = body.get("key")  # ty error here
        if isinstance(value, str):
            return value
    return None
```

### Error Output

```
error[invalid-argument-type]: Argument to bound method `get` is incorrect
  --> test.py:6:26
   |
 4 |         # ty reveals body as `Top[dict[Unknown, Unknown]]`
 5 |         # but dict.get() fails with "Expected Never, found Literal['key']"
 6 |         value = body.get("key")  # ty error here
   |                          ^^^^^ Expected `Never`, found `Literal["key"]`
   |
info: Matching overload defined here
    --> stdlib/builtins.pyi:3015:9
     |
3013 |     # Positional-only in dict, but not in MutableMapping
3014 |     @overload  # type: ignore[override]
3015 |     def get(self, key: _KT, default: None = None, /) -> _VT | None:
     |         ^^^       -------- Parameter declared here
3016 |         """Return the value for key if key is in the dictionary, else default."""
     |
info: Non-matching overloads for bound method `get`:
info:   (self, key: _KT@dict, default: _VT@dict, /) -> _VT@dict
info:   (self, key: _KT@dict, default: _T@get, /) -> _VT@dict | _T@get
```

### Expected Behavior

Pyright narrows `body` to `dict[Unknown, Unknown]` and allows `.get()` calls with any string key. ty should allow the same.

### ty Version

ty 0.0.9 (f1652f05d 2026-01-05)

---

_Closed by @yilei on 2026-01-06 22:56_

---

_Comment by @yilei on 2026-01-06 22:57_

I'm so sorry, I did not mean to create this issue.

---

_Closed by @yilei on 2026-01-06 23:14_

---
