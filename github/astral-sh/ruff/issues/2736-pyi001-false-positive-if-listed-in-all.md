---
number: 2736
title: "PYI001: false positive if listed in `__all__`"
type: issue
state: closed
author: spaceone
labels:
  - question
assignees: []
created_at: 2023-02-10T19:55:54Z
updated_at: 2023-02-26T23:25:00Z
url: https://github.com/astral-sh/ruff/issues/2736
synced_at: 2026-01-07T13:12:14-06:00
---

# PYI001: false positive if listed in `__all__`

---

_Issue opened by @spaceone on 2023-02-10 19:55_

`foo.pyi`:
```
from typing import TypeVar

__all__ = ['T',]

T = TypeVar("T")
```

as `T` is part of `__all__` it shouldn't be detected as `PYI001`

---

_Comment by @charliermarsh on 2023-02-10 20:04_

It looks like the upstream extension doesn't respect this. Is it intentional? Can you follow-up there?

---

_Label `question` added by @charliermarsh on 2023-02-10 20:04_

---

_Comment by @spaceone on 2023-02-10 20:56_

→ https://github.com/PyCQA/flake8-pyi/issues/347

---

_Comment by @charliermarsh on 2023-02-11 03:06_

Thanks! Not opposed to fixing, just want to make sure any deviations from upstream are intentional.

---

_Comment by @AlexWaygood on 2023-02-26 17:22_

FYI I closed the flake8-pyi issue as "wontfix" (but it's obviously your prerogative if you decide you'd prefer to deviate from our behaviour in ruff :)

See the flake8-pyi issue @spaceone linked to above for my reasoning.

---

_Comment by @charliermarsh on 2023-02-26 23:24_

Thanks @AlexWaygood for the thoughtful reply in there -- what you wrote makes sense to me, I'm gonna close out here too.

---

_Closed by @charliermarsh on 2023-02-26 23:24_

---

_Closed by @charliermarsh on 2023-02-26 23:25_

---
