```yaml
number: 1018
title: dedicated support for Django
type: issue
state: open
author: aaschenbrener
labels:
  - library
assignees: []
created_at: 2025-08-16T04:16:20Z
updated_at: 2026-01-08T18:19:26Z
url: https://github.com/astral-sh/ty/issues/1018
synced_at: 2026-01-10T01:56:40Z
```

# dedicated support for Django

---

_Issue opened by @aaschenbrener on 2025-08-16 04:16_

### Summary

This is an expansion on the known issue:  No implicit import of submodules (https://github.com/astral-sh/ty/issues/133). I've imported Django models explicitly from their submodules. There is an issue with dynamic model attributes like objects and id that Ty doesn’t recognize.

I’m encountering persistent unresolved-attribute errors when running Ty static type checker on Django models, specifically on standard Django model attributes such as id, demo_name, and objects. These errors occur even though the Django model is properly defined, registered, explicitly imported and confirmed to have those attributes in the Django shell.

Steps to reproduce:

Define a Django model with fields such as id, demo_name in apps/sample/models/example.py.

Confirm the model loads and works correctly inside the Django shell:

```py
from apps.sample.models.example import Demo
from django.db import models

issubclass(Demo, models.Model)  # Returns True
Demo._meta.get_fields()  # Returns expected model fields including 'id'
Demo.objects.all()  # Executes without error
```

Run Ty on a test file that uses this model, e.g. test_ty_model.py:

```py
def test_model_fields(m: Demo):
    reveal_type(m.id)
    reveal_type(m.demo_name)
    reveal_type(m.save)
```

Ty reports errors like:

```
error[unresolved-attribute]: Type `Demo` has no attribute `id`
error[unresolved-attribute]: Type `Demo` has no attribute `objects`
```

I have tried running Ty with explicit environment variables to load Django settings, but the errors persist:

```
PYTHONPATH=. DJANGO_SETTINGS_MODULE=proj.settings.dev ty check test_ty_model.py
```

Expected behavior:
Ty should recognize standard Django model attributes (id, objects, model fields) and when the Django environment is correctly configured.

Actual behavior:
Ty reports unresolved-attribute errors for these standard model attributes, even though Django shell confirms their existence.

Environment:

Ty version: 0.0.1-alpha.18 (d697cc092 2025-08-14)
Python version: 3.13.6
Django version: 5.2.5
OS: macOS 15.6 24G84


Project structure:

```
project-root/
├── manage.py
├── test_ty_model.py             
├── apps/
│   ├── sample/
│   │   ├── models/
│   │   │   └── example.py
├── proj/
│   ├── settings/
│   │   └── base.py
│   │   └──  dev.py
```

### Version

0.0.1-alpha.18 (d697cc092 2025-08-14)

---

_Comment by @carljm on 2025-08-16 14:51_

Thanks for the report! At the moment we don't have any dedicated support for Django, and due to Django's use of dynamic metaclass modifications, some attributes of Django's models aren't visible to a type-checker unless it implements dedicated Django support.

I'm a bit confused by the mention of using `django = true` in `.ty.toml`, since no such option exists (or has ever existed) in ty; seems like maybe something hallucinated by an LLM?

We would like to add some kind of Django support in future; will keep this issue open to track that.

---

_Label `feature` added by @carljm on 2025-08-16 14:51_

---

_Comment by @MichaReiser on 2025-08-16 14:57_

We also don't support `.ty.toml`. The configuration file must be named `ty.toml` and the `src` option is `src.root = ["apps", "proj"]`. Python path is under `environment`, etc. See https://docs.astral.sh/ty/reference/configuration/#environment

---

_Comment by @aaschenbrener on 2025-08-18 14:06_

Thank you for the update. Yes, I was having trouble with django and difficulty figuring out how ty.toml could help and attempted to  on an LLM to help configure. It seems to have hallucinated a lot. I've removed the ty.toml file completely for now. 

---

_Label `library` added by @AlexWaygood on 2025-11-04 22:29_

---

_Label `feature` removed by @AlexWaygood on 2025-11-04 22:29_

---

_Comment by @VaishnavGhenge on 2025-12-18 06:36_

+1. Django is one of the most popular Python frameworks, and these unresolved-attribute errors on model fields make ty unusable for Django projects without sprinkling # ty: ignore everywhere.
Would love to see either native support for Django's model metaclass patterns or a plugin system for django-stubs integration.

@carljm 

---

_Comment by @ronaldorcampos on 2025-12-19 22:06_

I have a lot of errors of the type

Attribute `id` may be missing on object of type `Unknown | ForeignKey` - accessing id in a FK
Attribute `save` may be missing on object of type `Unknown | ForeignKey` - accessing save in a FK
Element `BigIntegerField` of this union is not assignable to `int` - passing pk/id to a function that takes int 
Attribute `hour` may be missing on object of type `Unknown | DateTimeField` - accessing hour of a datetime field
No argument provided for required parameter `cls` of function `as_manager` - overriding objects manager

---

_Comment by @beaugunderson on 2025-12-19 22:14_

it does sound like there's a [commitment](https://astral.sh/blog/ty#:~:text=Following%20the%20Beta,and%20Django.) to future Django support, which I am REALLY looking forward to!

<img width="749" height="192" alt="Image" src="https://github.com/user-attachments/assets/d6933f75-702a-4112-9c32-80f18e686633" />

---

_Added to milestone `Stable` by @carljm on 2025-12-20 00:57_

---

_Comment by @ramtinabadi on 2025-12-26 13:12_

Just tinkering with ty, I had a workaround for Django's model using my old package, [django-hint](https://github.com/Vieolo/django-hint), which seems to work until a first-party support is added to ty.

Here is how you use it:

```python
from typing import cast
from django_hint import StandardModelType

class SomeModel(models.Model, StandardModelType['SomeModel']):
    name = cast('str', models.CharField(max_length=100))
```

Adding the `StandardModelType` to the class and casting each field, you wouldn't need to force ty to ignore the type check, and you'll have access to the query functions.

This is just a workaround until first-party support is added

---

_Renamed from "unresolved-attribute errors for Django model fields" to "dedicated support for Django" by @carljm on 2026-01-08 18:19_

---
