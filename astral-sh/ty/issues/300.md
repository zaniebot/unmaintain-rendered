```yaml
number: 300
title: "[ty] lint:unresolved-reference for globals"
type: issue
state: closed
author: mitsuhiko
labels: []
assignees: []
created_at: 2025-05-09T18:05:33Z
updated_at: 2025-05-09T20:35:26Z
url: https://github.com/astral-sh/ty/issues/300
synced_at: 2026-01-10T02:34:09Z
```

# [ty] lint:unresolved-reference for globals

---

_Issue opened by @mitsuhiko on 2025-05-09 18:05_

### Summary

```python
_foo = False

def stuff():
    global _foo
    if not _foo:
        _foo = True
```

Gives the following type check warning:

```
warning: lint:unresolved-reference: Name `_foo` used when not defined
 --> test.py:5:12
  |
3 | def stuff():
4 |     global _foo
5 |     if not _foo:
  |            ^^^^
6 |         _foo = True
  |
info: `lint:unresolved-reference` is enabled by default
```

### Version

ty 0.0.0-alpha.7 (905a3e1e5 2025-05-07)

---

_Comment by @ntBre on 2025-05-09 18:09_

I think this should be fixed by https://github.com/astral-sh/ruff/pull/17637, which should come out in alpha 8 momentarily!

https://github.com/astral-sh/ty/pull/298

---

_Comment by @AlexWaygood on 2025-05-09 20:35_

Confirmed that this is now fixed on the latest release: https://play.ty.dev/b5020ff1-6725-4b96-b23d-e0edcd73bb11

---

_Closed by @AlexWaygood on 2025-05-09 20:35_

---
