```yaml
number: 416
title: "`invalid-return-type` for global variable"
type: issue
state: closed
author: j178
labels:
  - narrowing
assignees: []
created_at: 2025-05-16T03:16:28Z
updated_at: 2025-05-16T03:24:20Z
url: https://github.com/astral-sh/ty/issues/416
synced_at: 2026-01-10T02:34:09Z
```

# `invalid-return-type` for global variable

---

_Issue opened by @j178 on 2025-05-16 03:16_

### Summary

It is common to use a function to return a global variable as a cache. Right now, `ty` is flagging errors on the type of the global variable. I also tried`mypy` and it passes.

```py
a = None

def f() -> int:
    global a
    if a is None:
        a = 1
    return a # error: Return type does not match returned value: expected `int`, found `Literal[1] | Unknown | None`
```

[playgroud](https://play.ty.dev/8e122ec4-47c0-474d-b529-c9d6a44063c0)

### Version

ty 0.0.1-alpha.3 (144a26d44 2025-05-15)

---

_Label `narrowing` added by @carljm on 2025-05-16 03:18_

---

_Closed by @AlexWaygood on 2025-05-16 03:24_

---
