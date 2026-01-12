```yaml
number: 860
title: "`__file__` import typed as `str | None`"
type: issue
state: open
author: sinon
labels:
  - runtime semantics
assignees: []
created_at: 2025-07-21T09:25:38Z
updated_at: 2025-12-18T23:12:06Z
url: https://github.com/astral-sh/ty/issues/860
synced_at: 2026-01-12T15:54:24Z
```

# `__file__` import typed as `str | None`

---

_@sinon_

### Summary

```python
import typing
import os 
from module import __file__ as module_path

typing.reveal_type(module_path). # str | None
os.path.abspath(module_path) # No overload of function `abspath` matches arguments
```

https://play.ty.dev/4dd59d9f-5dbe-433e-b370-67bbd5575fb2

`mypy` and `pyright` both detect the successful import and type the value as `str`

### Version

ty 0.0.1-alpha.15 (0369a3598 2025-07-18)

---

_Label `question` added by @MichaReiser on 2025-07-21 09:27_

---

_Comment by @sharkdp on 2025-07-21 09:36_

Thank you for reporting this.

`__file__` is annotated as having type `str | None` in [typeshed](https://github.com/python/typeshed/blob/ae1b471e5b96d34182b781700b9e277a369f766d/stdlib/types.pyi#L350) because there are modules for which `__file__` is actually `None`, like namespace modules.

That said, we could probably make an attempt to detect if the `__file__` attribute on a given module is `str` or `None`, similar to how we already do this for implicit global uses of `__file__` (https://github.com/astral-sh/ruff/pull/18071).

---

_Label `question` removed by @sharkdp on 2025-07-21 09:37_

---

_Label `runtime semantics` added by @sharkdp on 2025-07-21 09:37_

---

_Comment by @AlexWaygood on 2025-12-18 12:42_

See https://github.com/astral-sh/ty/issues/2063 for a similar issue regarding `__package__` in the global namespace

---

_Added to milestone `Stable` by @carljm on 2025-12-18 23:12_

---
