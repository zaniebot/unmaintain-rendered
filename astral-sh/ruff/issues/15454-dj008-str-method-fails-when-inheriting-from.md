```yaml
number: 15454
title: "DJ008 (`__str__` method) fails when inheriting from multiple models, and str only defined in other module"
type: issue
state: open
author: zyzzyxdonta
labels:
  - rule
  - type-inference
assignees: []
created_at: 2025-01-13T12:12:39Z
updated_at: 2026-01-15T14:27:09Z
url: https://github.com/astral-sh/ruff/issues/15454
synced_at: 2026-01-15T14:51:08Z
```

# DJ008 (`__str__` method) fails when inheriting from multiple models, and str only defined in other module

---

_@zyzzyxdonta_

This is related to #9242.

When a Django model inherits from an abstract model in the same module and a model in a different module, the `__str__` from the other module is not recognized and trips DJ008.

This is a minimal example that reproduces this behaviour:

[ruff-bug-repro.zip](https://github.com/user-attachments/files/18396513/ruff-bug-repro.zip)

```
.
├── mypackage
│   ├── core
│   │   ├── __init__.py
│   │   └── models.py
│   ├── __init__.py
│   └── resources
│       ├── __init__.py
│       └── models.py
└── pyproject.toml
```

Core models:

```python
from django.db import models

class Thing(models.Model):
    label = models.TextField()

    def __str__(self):
        return self.label or str(self.pk)
```

Resources models:

```python
from django.db import models

from mypackage.core.models import Thing


class Resource(models.Model):
    resource_id = models.AutoField(primary_key=True)

    class Meta:
        abstract = True


class Job(Resource, Thing):
    job_id = models.TextField()
```

Output from ruff 0.9.1:

```
mypackage/resources/models.py:13:7: DJ008 Model does not define `__str__` method
   |
13 | class Job(Resource, Thing):
   |       ^^^ DJ008
14 |     job_id = models.TextField()
   |

Found 1 error.
```

Thanks!

---

_Label `type-inference` added by @dylwil3 on 2025-01-13 12:22_

---

_Comment by @dhruvmanila on 2025-01-15 06:21_

I wonder if we should ignore this rule when the subclass is coming from a different file to reduce the false positive but that does come with the possibility of increasing the false negative.

---

_Label `rule` added by @MichaReiser on 2025-01-15 07:10_

---

_Comment by @tuttle on 2026-01-15 14:27_

We encountered this problem when creating Django models from abstract classes

```python
from django_extensions.db.models import TimeStampedModel

class SentNotification(TimeStampedModel):
    ...
```

This is very common in our codebase. The `DJ008` or `DJ012` rule don't not trigger at all and cannot detect missing `__str__` or bad ordering.

A similar issue is addressed in issues:
- https://github.com/astral-sh/ruff/issues/13664
- https://github.com/astral-sh/ruff/issues/12865

A workaround would be to have an option in `pyproject.toml` to **define a list of classes that can also serve as model bases** (aside from the standard `django.db.models.Model`):

```
[tool.ruff.lint.django]
additional-abstract-model-classes = [
    "django_extensions.db.models.TimeStampedModel",
]
```


---
