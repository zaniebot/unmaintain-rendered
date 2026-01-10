```yaml
number: 6730
title: "`ctypes.WinError()` returns false positive for RSE102"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-08-21T14:47:04Z
updated_at: 2023-12-06T17:03:38Z
url: https://github.com/astral-sh/ruff/issues/6730
synced_at: 2026-01-10T11:09:48Z
```

# `ctypes.WinError()` returns false positive for RSE102

---

_Issue opened by @charliermarsh on 2023-08-21 14:47_

Raising [`ctypes.WinError()`](https://docs.python.org/3/library/ctypes.html#ctypes.WinError) also triggers this. There is a [code snippet here](https://code.activestate.com/recipes/577972-disk-usage/) for example.

_Originally posted by @tdulcet in https://github.com/astral-sh/ruff/issues/5416#issuecomment-1686457590_


---

_Label `bug` added by @charliermarsh on 2023-08-21 14:47_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-21 14:47_

---

_Closed by @charliermarsh on 2023-08-21 14:57_

---

_Comment by @tdulcet on 2023-11-09 11:07_

This issue is occurring again. It seems to have been regressed by #8231.

---

_Comment by @tdulcet on 2023-12-06 13:22_

@charliermarsh - Friendly ping regarding the above comment, as the regression is still present in v0.1.7.

---

_Comment by @charliermarsh on 2023-12-06 16:34_

Oh thanks @tdulcet. Will take a look.

---

_Comment by @charliermarsh on 2023-12-06 17:03_

Fixed!

---
