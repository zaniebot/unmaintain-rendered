---
number: 19154
title: "DJ008 incorrectly reporting missing `__str__` method for abstract class with type annotations"
type: issue
state: closed
author: CarrotManMatt
labels:
  - bug
  - rule
  - help wanted
assignees: []
created_at: 2025-07-05T15:05:35Z
updated_at: 2025-07-11T16:51:01Z
url: https://github.com/astral-sh/ruff/issues/19154
synced_at: 2026-01-07T13:12:16-06:00
---

# DJ008 incorrectly reporting missing `__str__` method for abstract class with type annotations

---

_Issue opened by @CarrotManMatt on 2025-07-05 15:05_

### Summary

When a Django model class is marked as abstract the lack of a `__str__` method is not flagged. This is correct behaviour.

```python
from django.db import models

class Foo(models.Model)
    class Meta:
        abstract = True
```

When a type annotation is added to the meta class, the identification of the model being abstract is lost, so the DJ008 diagnostic *incorrectly* occurs.

```python
from typing import ClassVar
from django.db import models
from django_stubs_ext.db.models import TypedModelMeta


class Foo(models.Model):
    class Meta(TypedModelMeta):
        abstract: ClassVar[bool] = True
```

### Version

0.12.2

---

_Label `bug` added by @ntBre on 2025-07-07 21:22_

---

_Label `rule` added by @ntBre on 2025-07-07 21:22_

---

_Label `help wanted` added by @ntBre on 2025-07-07 21:22_

---

_Comment by @ntBre on 2025-07-07 21:23_

Thanks for the report! I think we should also check for a `Stmt::AnnAssign` here:

https://github.com/astral-sh/ruff/blob/2f8bc9a30bbcb3839fa0a5031e0bbae329210e53/crates/ruff_linter/src/rules/flake8_django/rules/model_without_dunder_str.rs#L99-L101

---

_Referenced in [astral-sh/ruff#19221](../../astral-sh/ruff/pulls/19221.md) on 2025-07-08 23:59_

---

_Closed by @MichaReiser on 2025-07-11 16:51_

---
