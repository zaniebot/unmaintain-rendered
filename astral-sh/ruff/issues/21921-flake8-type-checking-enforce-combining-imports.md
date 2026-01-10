```yaml
number: 21921
title: "`flake8-type-checking`: enforce combining imports from inside and outside a `TYPE_CHECKING` block (reverse of `strict`)"
type: issue
state: closed
author: GideonBear
labels: []
assignees: []
created_at: 2025-12-11T16:14:55Z
updated_at: 2025-12-11T18:25:43Z
url: https://github.com/astral-sh/ruff/issues/21921
synced_at: 2026-01-10T11:10:00Z
```

# `flake8-type-checking`: enforce combining imports from inside and outside a `TYPE_CHECKING` block (reverse of `strict`)

---

_Issue opened by @GideonBear on 2025-12-11 16:14_

### Summary

When using `tool.ruff.lint.flake8-type-checking.strict = false` (the default), ruff will not split up imports into a `TYPE_CHECKING` block:
```py
from __future__ import annotations

from m import A, B

a = A()
b: B
```
```console
$ \cat pyproject.toml
tool.ruff.lint.flake8-type-checking.strict = false
$ ruff check main.py --select TC
All checks passed!
$ nano pyproject.toml 
$ \cat pyproject.toml
tool.ruff.lint.flake8-type-checking.strict = true
$ ruff check main.py --select TC
TC002 Move third-party import `m.B` into a type-checking block
etc.
```

`strict = true` is great for consistency. However when using `strict = false`, there is no way to enforce having these imports combined, making it impossible to enforce consistency.

Current behavior:
```py
from __future__ import annotations

from typing import TYPE_CHECKING

from m import A

if TYPE_CHECKING:
    from m import B

a = A()
b: B
```
```console
$ ruff check main.py --select TC
All checks passed!
```
Desired behavior: suggest combining the imports (to what it was in the first example).

---

_Comment by @GideonBear on 2025-12-11 16:16_

Soft ping to @Daverball, thoughts?

---

_Comment by @ntBre on 2025-12-11 17:15_

If I understand correctly, this is a duplicate of https://github.com/astral-sh/ruff/issues/19548.

---

_Comment by @Daverball on 2025-12-11 17:56_

Looks like a duplicate to me as well. Overall I don't dislike the idea, but it definitely can't be a boolean setting at this point. We don't want to cause arguably unnecessary churn in either direction, unless people specifically opt into it for aesthetics/consistency. So it would have to be a three-stage setting, either you prefer imports staying together, you prefer all the typing only imports staying in type checking blocks, even if it means splitting up imports or you have no preference and ruff  will only nag you when there's a tangible benefit to moving the import beyond aesthetics/consistency.

---

_Comment by @GideonBear on 2025-12-11 17:58_

> If I understand correctly, this is a duplicate of [#19548](https://github.com/astral-sh/ruff/issues/19548).

It is, looks like I missed that. Thanks!

---

_Closed by @GideonBear on 2025-12-11 17:58_

---

_Comment by @GideonBear on 2025-12-11 17:58_

(accidentally closed as completed, meant to close as duplicate)

---

_Comment by @vivodi on 2025-12-11 18:23_

> (accidentally closed as completed, meant to close as duplicate)

You can still reopen and close it as duplicate.

---

_Closed by @GideonBear on 2025-12-11 18:25_

---

_Comment by @GideonBear on 2025-12-11 18:25_

> > (accidentally closed as completed, meant to close as duplicate)
> 
> You can still reopen and close it as duplicate.

Huh, the button *was* greyed-out for me, I assumed I didn't have permission to do that. Thx

---
