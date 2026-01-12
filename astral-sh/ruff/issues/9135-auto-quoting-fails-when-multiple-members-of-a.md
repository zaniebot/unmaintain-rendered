```yaml
number: 9135
title: Auto-quoting fails when multiple members of a union are quoted
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-12-14T18:12:37Z
updated_at: 2023-12-14T19:46:37Z
url: https://github.com/astral-sh/ruff/issues/9135
synced_at: 2026-01-12T15:54:48Z
```

# Auto-quoting fails when multiple members of a union are quoted

---

_@charliermarsh_

Given:

```python
from django.contrib.auth.models import AbstractBaseUser, AnonymousUser

def foo(
    self, user: AbstractBaseUser | AnonymousUser, view: 'type[CondorBaseViewSet]'
):
    pass
```

Running `cargo run -p ruff_cli -- check foo.py --select TCH --fix -n --unsafe-fixes` fails due to `assertion failed: start.raw <= end.raw`.

---

_Label `bug` added by @charliermarsh on 2023-12-14 18:12_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-14 19:41_

---

_Closed by @charliermarsh on 2023-12-14 19:46_

---
