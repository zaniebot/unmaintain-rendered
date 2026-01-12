```yaml
number: 2240
title: "Treat Callable as having \"__name__\" attribute"
type: issue
state: closed
author: dchevell
labels: []
assignees: []
created_at: 2025-12-27T21:49:40Z
updated_at: 2025-12-29T19:57:35Z
url: https://github.com/astral-sh/ty/issues/2240
synced_at: 2026-01-12T15:54:26Z
```

# Treat Callable as having "__name__" attribute

---

_@dchevell_

### Summary

`mypy` and `pyright` intentionally assume that `Callable` has a `__name__` attribute, but `ty` doesn't, and reports usage of `__name__` as an error:

```python
# err_report.py
from typing import Any
from collections.abc import Callable

def fn_name(fn: Callable[..., Any]) -> str:
    return fn.__name__
```

```shell
$ mypy err_report.py
Success: no issues found in 1 source file
$ pyright err_report.py
0 errors, 0 warnings, 0 informations
$ ty check err_report.py
error[unresolved-attribute]: Object of type `(...) -> Any` has no attribute `__name__`
 --> err_report.py:6:12
  |
5 | def fn_name(fn: Callable[..., Any]) -> str:
6 |     return fn.__name__
  |            ^^^^^^^^^^^
  |
help: Function objects have a `__name__` attribute, but not all callable objects are functions
info: rule `unresolved-attribute` is enabled by default

Found 1 diagnostic
```

My understanding (from python/mypy#14392) is that `ty` is technically correct here, but that mypy et al. are making an intentional special case here to avoid false positives. Can `ty` conform to the same behaviour?

### Version

ty 0.0.7 (cf82a04b5 2025-12-24)

---

_Comment by @MeGaGiGaGon on 2025-12-27 22:45_

Discussion about this is still ongoing, see #1495 and #491

---

_Closed by @MichaReiser on 2025-12-29 07:44_

---

_Reopened by @carljm on 2025-12-29 19:55_

---

_Closed by @carljm on 2025-12-29 19:55_

---

_Comment by @carljm on 2025-12-29 19:56_

For clarity -- the issue here is #1495. #491 is loosely related in that they are both about "unsound assumptions type checkers make about Callable types", but presence of `__name__` is not related to being a bound method descriptor.

---

_Comment by @carljm on 2025-12-29 19:57_

(FWIW, our current leaning in #1495 is not to copy the unsound assumption of mypy here, because we think ty can provide the tools to handle this soundly instead.)

---
