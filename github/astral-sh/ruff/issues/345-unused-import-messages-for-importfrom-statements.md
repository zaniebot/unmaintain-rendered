---
number: 345
title: "Unused import messages for `ImportFrom` statements are wonky"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2022-10-07T19:42:38Z
updated_at: 2022-10-07T19:58:44Z
url: https://github.com/astral-sh/ruff/issues/345
synced_at: 2026-01-07T13:12:14-06:00
---

# Unused import messages for `ImportFrom` statements are wonky

---

_Issue opened by @charliermarsh on 2022-10-07 19:42_

Example:

```py
resources/test/fixtures/F401.py:29:5: F401 `from models import Fruit, Nut, Vegetable.Vegetable, from models import Fruit, Nut, Vegetable.Nut, from models import Fruit, Nut, Vegetable.Fruit` imported but unused
```


---

_Label `bug` added by @charliermarsh on 2022-10-07 19:42_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-10-07 19:42_

---

_Comment by @charliermarsh on 2022-10-07 19:44_

Wait, nevermind, this was a bug on my tree-sitter branch :)

---

_Closed by @charliermarsh on 2022-10-07 19:44_

---

_Comment by @charliermarsh on 2022-10-07 19:58_

I did improve this a bit in #346 though.

---

_Referenced in [codegrande/ruff#4](../../codegrande/ruff/pulls/4.md) on 2024-05-17 07:07_

---
