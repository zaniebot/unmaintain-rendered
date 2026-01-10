```yaml
number: 1097
title: unresolved-attribute error for Django model DoesNotExist exception
type: issue
state: closed
author: maver1ck
labels: []
assignees: []
created_at: 2025-08-26T09:31:37Z
updated_at: 2025-08-26T12:54:48Z
url: https://github.com/astral-sh/ty/issues/1097
synced_at: 2026-01-10T02:06:24Z
```

# unresolved-attribute error for Django model DoesNotExist exception

---

_Issue opened by @maver1ck on 2025-08-26 09:31_

### Summary

Ty reports `unresolved-attribute` errors when trying to access Django model's dynamically generated `DoesNotExist` exception class. This is a common pattern in Django code where each model automatically gets a `DoesNotExist` exception class for handling object not found scenarios.

### Steps to reproduce

I've created a minimal reproduction case at: https://github.com/maver1ck/ty-bug

1. Create a Django model that extends `AbstractUser`:
```python
from django.contrib.auth.models import AbstractUser
from django.db import models

class CustomUser(AbstractUser):
    email = models.EmailField(unique=True)
```

2. Try to catch the model's `DoesNotExist` exception:
```python
try:
    user = CustomUser.objects.get(username=username)
except CustomUser.DoesNotExist:  # <- ty reports error here
    raise CommandError(f"No user found!") from None
```

3. Run `uvx ty check`

### Expected behavior

Ty should recognize that Django models automatically have a `DoesNotExist` exception class available as a class attribute.

### Actual behavior

```
error[unresolved-attribute]: Type `<class 'CustomUser'>` has no attribute `DoesNotExist`
  --> apps/users/management/commands/promote_user_to_superuser.py:16:16
   |
14 |         try:
15 |             user = CustomUser.objects.get(username=username)
16 |         except CustomUser.DoesNotExist:
   |                ^^^^^^^^^^^^^^^^^^^^^^^
17 |             raise CommandError(f"No user with username/email {username} found!") from None
18 |         user.is_superuser = True
   |
info: rule `unresolved-attribute` is enabled by default
```

### Environment

- Ty version: Latest (using `uvx ty check`)
- Python version: 3.12+  
- Django version: 5.2.5
- OS: macOS

### Additional context

This is related to issue #1018 but focuses specifically on the `DoesNotExist` exception class rather than model fields. In Django, every model class automatically gets:
- A `DoesNotExist` exception class
- A `MultipleObjectsReturned` exception class
- Various manager attributes like `objects`

These are created dynamically by Django's metaclass system and are fundamental to Django development patterns.

The reproduction repository contains the minimal case to reproduce this specific issue.

---

_Comment by @burrscurr on 2025-08-26 12:01_

I don't know whether `ty` should pick up that django models have a `DoesNotExist` attribute, and with


```py
from django.db import models


class CustomUser(models.Model): ...


CustomUser.DoesNotExist  # no error when using django-stubs
```

and `uv run --no-project --with django==5.2.5 --with ty==0.0.1a19 ty check` I can reproduce what you describe. As far as I know, django does not really use type hints in its code, and external type hints are provided by projects like [`django-stubs`](https://pypi.org/project/django-stubs/). With `uv run --no-project --with django==5.2.5 --with django-stubs==5.2.2 --with ty==0.0.1a19 ty check` I don't get an unresolved attribute warning for the exception, so this might be a workaround for you.

---

_Comment by @maver1ck on 2025-08-26 12:54_

Thanks @burrscurr.
That's the solution :)

---

_Closed by @maver1ck on 2025-08-26 12:54_

---
