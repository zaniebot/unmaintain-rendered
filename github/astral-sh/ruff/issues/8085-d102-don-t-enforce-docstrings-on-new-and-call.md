---
number: 8085
title: "`D102` - Don't enforce docstrings on `__new__` and `__call__`"
type: issue
state: open
author: DetachHead
labels:
  - docstring
  - needs-decision
assignees: []
created_at: 2023-10-20T02:10:27Z
updated_at: 2024-04-02T12:39:46Z
url: https://github.com/astral-sh/ruff/issues/8085
synced_at: 2026-01-07T13:12:15-06:00
---

# `D102` - Don't enforce docstrings on `__new__` and `__call__`

---

_Issue opened by @DetachHead on 2023-10-20 02:10_

re-raising https://github.com/PyCQA/pydocstyle/issues/515 here:

> This should be considered a magic method (controlled by `D105`), or at least have a different error code (which may be `D107` together with `__init__` or even a new one).

---

_Renamed from "Don't enforce docstrings on `__new__`" to "`D102` - Don't enforce docstrings on `__new__`" by @DetachHead on 2023-10-20 02:10_

---

_Renamed from "`D102` - Don't enforce docstrings on `__new__`" to "`D102` - Don't enforce docstrings on `__new__` and `__call__`" by @DetachHead on 2023-10-20 02:11_

---

_Label `docstring` added by @charliermarsh on 2023-10-20 21:21_

---

_Label `needs-decision` added by @charliermarsh on 2023-10-20 21:21_

---

_Comment by @pya on 2024-03-30 14:30_

`pycodestyle` doesn't complain about a `__new__` method without a docstring. Ruff should be consistent and not consider `__new__` an ordinary public method that requires a dosctring.

---

_Comment by @dhruvmanila on 2024-04-01 08:34_

@pya  Apologies if it's just a typo but I think the plugin which is being referenced here is `pydocstyle` ("doc") and not `pycodestyle` ("code"). `pycodestyle` doesn't have rules around [docstring convention](https://pycodestyle.pycqa.org/en/latest/intro.html#id3).

---

_Comment by @pya on 2024-04-02 12:39_

@dhruvmanila Thanks for the hint. Mixed up "code" and "doc".

---

_Referenced in [astral-sh/ruff#13079](../../astral-sh/ruff/issues/13079.md) on 2024-08-23 13:20_

---

_Referenced in [materialsproject/pymatgen#4493](../../materialsproject/pymatgen/pulls/4493.md) on 2025-09-11 14:14_

---
