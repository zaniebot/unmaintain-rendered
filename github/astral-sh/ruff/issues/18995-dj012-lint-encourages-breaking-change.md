---
number: 18995
title: DJ012  lint encourages breaking change
type: issue
state: open
author: DMunkei
labels:
  - rule
assignees: []
created_at: 2025-06-27T19:25:06Z
updated_at: 2025-07-07T13:40:18Z
url: https://github.com/astral-sh/ruff/issues/18995
synced_at: 2026-01-07T13:12:16-06:00
---

# DJ012  lint encourages breaking change

---

_Issue opened by @DMunkei on 2025-06-27 19:25_

### Summary

Since the ordering is important when it comes to managers in Django, reordering might introduce unexpected behaviour. In this case the lint `DJ012` was getting triggered even though both objects are `Managers`.

This is a minimal example to reproduce it.
```py
class NotDeletedObjectsManager(Manager):
    """Returns only those objects that are do not have delete_flag=1"""

    def get_queryset(self):
        return super().get_queryset().exclude(delete_flag=1)


class QuestBaseModel(models.Model):
    objects = NotDeletedObjectsManager()
    all_objects = models.Manager() 
    class Meta:
        abstract = True


class MyModel(QuestBaseModel):...
```

Here's the output of the command `ruff check`
```bash
api/utils/models.py:17:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: field declaration should come before manager declaration
   |
16 |     objects = NotDeletedObjectsManager()
17 |     all_objects = models.Manager()
   |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ DJ012
18 |
19 |     class Meta:
```
Here is additional information regarding the versions of the programs installed.
```
Ruff version: ruff 0.12.0
Python version: 3.12.9
Django version: 5.0
```


### Version

0.12.0

---

_Comment by @MeGaGiGaGon on 2025-06-27 22:47_

Example in playground: https://play.ruff.rs/1c3aba5d-1917-4581-85ea-609794264552
My guess would be that a setting should be added to allow extending what ruff considers a "manager".

---

_Comment by @DMunkei on 2025-06-28 07:36_

Is there no way to make it realise that both are Managers?

---

_Comment by @MeGaGiGaGon on 2025-06-28 07:47_

Not currently. Looking at the code for `DJ012` only an assignment to the name "object" is considered a manager declaration.
https://github.com/astral-sh/ruff/blob/90cb0d3a7b0d1b9528854dad64dd17330bfd5f36/crates/ruff_linter/src/rules/flake8_django/rules/unordered_body_content_in_model.rs#L153-L186
I'm not familiar with how Django works, but there probably either needs to be an extension to the codes logic to account for other types of manager declarations, and/or a setting for customizing this behavior.

---

_Comment by @DMunkei on 2025-06-28 17:14_

In this case the order of the managers matters when queries are executed. 
[Documentation regarding default manager definition](https://docs.djangoproject.com/en/5.2/topics/db/managers/#django.db.models.Model._default_manager)

---

_Comment by @ntBre on 2025-06-30 13:33_

I think we should at least try to infer the type to see if it's a `Manager` instead of going by the assigned name. It could be a bit tedious to have to configure each one. We'd need multi-file analysis to do the inference robustly, but we can at least handle the single-file case for now.

---

_Label `bug` added by @ntBre on 2025-06-30 13:33_

---

_Label `rule` added by @ntBre on 2025-06-30 13:33_

---

_Comment by @DMunkei on 2025-06-30 15:52_

From an outside perspective, not knowing how it's done under the hood. I agree with @ntBre , that inference would be less tedious and moreover the type information should be available. No? 

---

_Label `bug` removed by @MichaReiser on 2025-07-07 12:44_

---

_Comment by @MichaReiser on 2025-07-07 12:47_

Maybe a noob question but I'm not using Django myself. Is there a naming convention for manager fields? Looking at the implementation here

https://github.com/astral-sh/ruff/blob/2d224e6096401e0a512d49899d2e6374c932c979/crates/ruff_linter/src/rules/flake8_django/rules/unordered_body_content_in_model.rs#L156-L169

It seems all we do to identify a manager field is to test if it is named `objects`. Could we test if the field ends with `_objects` instead of testing for an exact match?



---

_Comment by @DMunkei on 2025-07-07 13:40_

Per default, model classes _do not_ require you to define a `objects` field. If you happen to define one, the naming convention in the end is irrelevant. I do not believe there's a naming convention, I might be wrong though.

---
