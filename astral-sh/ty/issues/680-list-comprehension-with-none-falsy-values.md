```yaml
number: 680
title: list comprehension with None (falsy) values filtered by condition
type: issue
state: closed
author: pawamoy
labels:
  - narrowing
  - control flow
assignees: []
created_at: 2025-06-18T14:44:54Z
updated_at: 2025-06-25T09:30:29Z
url: https://github.com/astral-sh/ty/issues/680
synced_at: 2026-01-12T15:54:23Z
```

# list comprehension with None (falsy) values filtered by condition

---

_@pawamoy_

### Summary

```python
import os


def func(param: str) -> str:
    return param


l = [func(val) for var in ("HELLO", "WORLD") if (val := os.getenv(var))]
```

```console
% uvx ty check t.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[invalid-argument-type]: Argument to function `func` is incorrect
 --> t.py:8:11
  |
8 | l = [func(val) for var in ("HELLO", "WORLD") if (val := os.getenv(var))]
  |           ^^^ Expected `str`, found `str | None`
  |
info: Function defined here
 --> t.py:4:5
  |
4 | def func(param: str) -> str:
  |     ^^^^ ---------- Parameter declared here
5 |     return param
  |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

Here `func` cannot ever be called with `None` since such falsy values are filtered by the list comprehension condition `if (val := os.getenv(var))`.

### Version

ty 0.0.1-alpha.11

---

_Comment by @AlexWaygood on 2025-06-18 14:59_

haven't looked in depth but this might be the same underlying issue as #626

---

_Label `control flow` added by @carljm on 2025-06-18 18:54_

---

_Label `narrowing` added by @carljm on 2025-06-18 18:54_

---

_Comment by @sharkdp on 2025-06-23 06:35_

> haven't looked in depth but this might be the same underlying issue as [#626](https://github.com/astral-sh/ty/issues/626)

I don't think so? There's no boolean operator here, and something like the following works just fine:
```py
if val := os.getenv("HELLO"):
    func(val)
```

It looks like we don't record narrowing constraints for generator expressions at all ([ref](https://github.com/astral-sh/ruff/blob/e1809752269c65418f396671afac042bec3b7a86/crates/ty_python_semantic/src/semantic_index/builder.rs#L857-L859))?

---

_Closed by @sharkdp on 2025-06-25 09:30_

---
