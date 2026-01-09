---
number: 2892
title: "[flake8-django] DJ001 is not raised for models not directly inherited from Model"
type: issue
state: closed
author: konysko
labels:
  - bug
assignees: []
created_at: 2023-02-14T10:46:13Z
updated_at: 2023-02-15T15:54:17Z
url: https://github.com/astral-sh/ruff/issues/2892
synced_at: 2026-01-07T13:12:14-06:00
---

# [flake8-django] DJ001 is not raised for models not directly inherited from Model

---

_Issue opened by @konysko on 2023-02-14 10:46_

DJ001 is not raised for models which doesn't directly inherits from django.db.models.Model. The source of bug is in https://github.com/charliermarsh/ruff/blob/f7515739acc0a7b5c580bf05aac6055181739c65/crates/ruff/src/rules/flake8_django/rules/helpers.rs#L6 It should lookup whole mro for the Model class. 

Affected version: 0.0.246
Snippet reproducing bug:
```python
from django.db import models
from django.utils.translation import gettext_lazy as _
from utils.models import BaseModel


class ExampleModel(BaseModel):
    name = models.CharField(max_length=255, verbose_name=_('Name'), null=True)
    description = models.TextField(verbose_name=_('Description'))

    def __str__(self) -> str:
        return self.name
```


---

_Referenced in [astral-sh/ruff#2586](../../astral-sh/ruff/pulls/2586.md) on 2023-02-14 10:47_

---

_Comment by @charliermarsh on 2023-02-14 16:04_

I think in this case it would also be fine to skip the base model check. If a class has a `django.models.CharField`, that's good enough IMO.

---

_Label `bug` added by @charliermarsh on 2023-02-15 14:33_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-15 15:43_

---

_Referenced in [astral-sh/ruff#2928](../../astral-sh/ruff/pulls/2928.md) on 2023-02-15 15:50_

---

_Closed by @charliermarsh on 2023-02-15 15:54_

---
