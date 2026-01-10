```yaml
number: 1862
title: "False positive for `possibly-unresolved-reference` using walrus operator"
type: issue
state: closed
author: xoudini
labels: []
assignees: []
created_at: 2025-12-12T12:21:10Z
updated_at: 2025-12-12T12:24:29Z
url: https://github.com/astral-sh/ty/issues/1862
synced_at: 2026-01-10T01:55:00Z
```

# False positive for `possibly-unresolved-reference` using walrus operator

---

_Issue opened by @xoudini on 2025-12-12 12:21_

### Summary

Here's a minimal reproducible example of ty incorrectly reporting a possibly unresolved reference:

```python
def foo(x: int | None) -> int | None:
    return x


def bar(x: int | None, y: bool) -> int:
    if y and (_x := foo(x)) is not None:
        return _x
    return 0
```

The error itself:

```console
error[possibly-unresolved-reference]: Name `_x` used when possibly not defined
 --> tymre.py:7:16
  |
5 | def bar(x: int | None, y: bool) -> int:
6 |     if y and (_x := foo(x)) is not None:
7 |         return _x
  |                ^^
8 |     return 0
  |
info: rule `possibly-unresolved-reference` was selected in the configuration file
```

Without the `y` check (or swapping the order of the conditions) the check passes.

In this case `mypy 1.18.2` correctly type checks with `possibly-undefined` enabled.

### Version

0.0.1-alpha.33 (35e23aa48 2025-12-09)

---

_Comment by @xoudini on 2025-12-12 12:24_

Sorry, just noticed https://github.com/astral-sh/ty/issues/139 and https://github.com/astral-sh/ty/issues/626 after modifying my search terms in the issues, self-closing.

---

_Closed by @xoudini on 2025-12-12 12:24_

---
