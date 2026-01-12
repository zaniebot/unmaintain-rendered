```yaml
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
synced_at: 2026-01-12T15:54:40Z
```

# Unused import messages for `ImportFrom` statements are wonky

---

_@charliermarsh_

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
