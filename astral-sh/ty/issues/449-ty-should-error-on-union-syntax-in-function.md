```yaml
number: 449
title: ty should error on Union syntax in function definition without annotations import in Python 3.9
type: issue
state: open
author: metaist
labels:
  - diagnostics
  - runtime semantics
assignees: []
created_at: 2025-05-19T13:27:43Z
updated_at: 2025-11-14T14:44:28Z
url: https://github.com/astral-sh/ty/issues/449
synced_at: 2026-01-12T15:54:23Z
```

# ty should error on Union syntax in function definition without annotations import in Python 3.9

---

_@metaist_

### Summary

Splitting this off from #437. Although `UnionType` was introduced in Python 3.10, it is possible to use the union syntax in function definitions (but not in type aliases or elsewhere) in Python 3.9 if you include `from __future__ import annotations`

`mypy` catches this; `ty` does not.

https://play.ty.dev/5c47f2d5-ef8c-4d2f-b227-f93178b29e57

```python
def f(x: int|str) -> None: ...
```

```bash
$ uvx --python 3.9 mypy --strict test.py
test.py:1: error: X | Y syntax for unions requires Python 3.10  [syntax]
Found 1 error in 1 file (checked 1 source file)

$ uvx ty check --python-version 3.9 test.py
All checks passed!
```
https://play.ty.dev/23b020e7-4d07-49d7-9671-9918244dad1c

```python
from __future__ import annotations
def f(x: int|str) -> None: ...
```

```bash
$ uvx --python 3.9 mypy --strict test.py
Success: no issues found in 1 source file

$ uvx ty check --python-version 3.9 test.py
All checks passed!
```

### Version

ty 0.0.1-alpha.5 (4ad13f2 2025-05-17)

---

_Label `diagnostics` added by @sharkdp on 2025-05-19 13:29_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-05-19 13:58_

---

_Added to milestone `Stable` by @carljm on 2025-11-14 14:44_

---
