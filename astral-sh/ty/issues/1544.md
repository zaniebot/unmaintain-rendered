```yaml
number: 1544
title: Bases of a dynamically created class are not recognized
type: issue
state: closed
author: m-pilia
labels: []
assignees: []
created_at: 2025-11-14T08:31:20Z
updated_at: 2025-11-14T08:58:45Z
url: https://github.com/astral-sh/ty/issues/1544
synced_at: 2026-01-10T02:06:25Z
```

# Bases of a dynamically created class are not recognized

---

_Issue opened by @m-pilia on 2025-11-14 08:31_

### Summary

Minimal example:
```python
class Foo:
    pass

x: type[Foo] = type("MyType", (Foo,), {})
```
Result:
```
$  ty check ty_repr.py
error[invalid-assignment]: Object of type `type` is not assignable to `type[Foo]`
 --> ty_repr.py:4:1
  |
2 |     pass
3 |
4 | x: type[Foo] = type("MyType", (Foo,), {})
  | ^
  |
info: rule `invalid-assignment` is enabled by default
```
Expected result: No error should be reported.

See also playground: https://play.ty.dev/5576a11d-e6af-49c8-a08e-fc29816acb44

### Version

ty 0.0.1-alpha.26 (b225fd8b4 2025-11-10)

---

_Closed by @AlexWaygood on 2025-11-14 08:58_

---

_Comment by @AlexWaygood on 2025-11-14 08:58_

Thanks! Please see #740

---
