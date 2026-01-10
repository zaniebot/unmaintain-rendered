---
number: 4485
title: RUF010 autofix error with dict variable
type: issue
state: closed
author: fennerm
labels:
  - bug
assignees: []
created_at: 2023-05-17T23:29:39Z
updated_at: 2023-05-18T16:40:38Z
url: https://github.com/astral-sh/ruff/issues/4485
synced_at: 2026-01-10T01:22:43Z
---

# RUF010 autofix error with dict variable

---

_Issue opened by @fennerm on 2023-05-17 23:29_

(ruff=v0.0.267)

Hopefully my last ticket today 🤞

Minimal example:
```
foo = {
    "x": 1
}

bar = f"{str(foo['x'])}"
```

Ruff correctly flags the error: `RUF010 Use conversion in f-string`

When running `ruff --fix` I get:
```
error: Autofix introduced a syntax error. Reverting all changes.
```

Many thanks for all your work on ruff!

---

_Comment by @charliermarsh on 2023-05-18 00:48_

I believe this is already fixed on `main` via #4423 -- will go out in the next release.

---

_Closed by @charliermarsh on 2023-05-18 00:48_

---

_Label `bug` added by @charliermarsh on 2023-05-18 00:48_

---

_Comment by @fennerm on 2023-05-18 16:37_

Ah missed that ticket somehow, thanks!

---

_Comment by @charliermarsh on 2023-05-18 16:40_

No prob!

---
