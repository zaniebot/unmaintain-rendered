---
number: 17935
title: More deprecated aliases of built-in exceptions
type: issue
state: open
author: DimitriPapadopoulos
labels:
  - rule
assignees: []
created_at: 2025-05-08T06:24:17Z
updated_at: 2025-05-09T19:38:41Z
url: https://github.com/astral-sh/ruff/issues/17935
synced_at: 2026-01-07T13:12:16-06:00
---

# More deprecated aliases of built-in exceptions

---

_Issue opened by @DimitriPapadopoulos on 2025-05-08 06:24_

### Summary

Update [os-error-alias (UP024)](https://docs.astral.sh/ruff/rules/os-error-alias/#os-error-alias-up024) for the first one:

* [`resource.error`](https://docs.python.org/3/library/resource.html#resource.error) is a deprecated alias of [`OSError`](https://docs.python.org/3/library/exceptions.html#OSError):
  > _Changed in version 3.3:_ Following [**PEP 3151**](https://peps.python.org/pep-3151/), this class was made an alias of [`OSError`](https://docs.python.org/3/library/exceptions.html#OSError).
* `ExecError` will be a deprecated alias of  [`RuntimeError`](https://docs.python.org/3/library/exceptions.html#RuntimeError), [starting with Python 3.14](https://docs.python.org/3/deprecations/index.html#pending-removal-in-python-3-16):
  > The `ExecError` exception has been deprecated since Python 3.14. It has not been used by any function in `shutil` since Python 3.4, and is now an alias of [`RuntimeError`](https://docs.python.org/3/library/exceptions.html#RuntimeError).



---

_Referenced in [astral-sh/ruff#17933](../../astral-sh/ruff/pulls/17933.md) on 2025-05-08 06:25_

---

_Label `rule` added by @ntBre on 2025-05-09 19:38_

---
