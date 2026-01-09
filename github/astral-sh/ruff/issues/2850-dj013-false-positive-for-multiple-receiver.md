---
number: 2850
title: "`DJ013`: False positive for multiple `@receiver` decorators"
type: issue
state: closed
author: ngnpope
labels:
  - bug
assignees: []
created_at: 2023-02-13T11:15:18Z
updated_at: 2023-02-13T14:57:13Z
url: https://github.com/astral-sh/ruff/issues/2850
synced_at: 2026-01-07T13:12:14-06:00
---

# `DJ013`: False positive for multiple `@receiver` decorators

---

_Issue opened by @ngnpope on 2023-02-13 11:15_

For the following:

```python
from django.db import models
from django.db.models.signals import post_save
from django.dispatch import receiver

class ModelA(models.Model):
    pass

class ModelB(models.Model):
    pass

@receiver(post_save, sender=ModelA)
@receiver(post_save, sender=ModelB)
def on_post_save(**kwargs):
    pass
```

I get the following:

```console
❯ ruff --version
ruff 0.0.246

❯ ruff --isolated --select DJ013 bug.py
bug.py:12:2: DJ013 `@receiver` decorator must be on top of all the other decorators
Found 1 error.
```

I expect no errors. When fixing, a test should also be added for multiple `@receiver` decorators with other decorators too to ensure all `@receiver` decorators are the outermost.

---

_Label `bug` added by @charliermarsh on 2023-02-13 14:37_

---

_Referenced in [astral-sh/ruff#2858](../../astral-sh/ruff/pulls/2858.md) on 2023-02-13 14:48_

---

_Closed by @charliermarsh on 2023-02-13 14:57_

---
