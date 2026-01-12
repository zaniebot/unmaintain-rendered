```yaml
number: 9137
title: Add support for auto-quoting when annotations contain quotes
type: issue
state: closed
author: charliermarsh
labels:
  - fixes
assignees: []
created_at: 2023-12-14T18:15:24Z
updated_at: 2024-10-23T11:04:04Z
url: https://github.com/astral-sh/ruff/issues/9137
synced_at: 2026-01-12T15:54:48Z
```

# Add support for auto-quoting when annotations contain quotes

---

_@charliermarsh_

Right now, given:

```python
from django.contrib.auth.models import AbstractBaseUser

def foo(
    self, user: AbstractBaseUser['int'], view: 'type[CondorBaseViewSet]'
):
    pass
```

We avoid providing a fix for `AbstractBaseUser` since it contains a quote in the expanded expression.

---

_Label `autofix` added by @charliermarsh on 2023-12-14 18:15_

---

_Closed by @dhruvmanila on 2024-10-23 11:04_

---
