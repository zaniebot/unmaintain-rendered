---
number: 7803
title: "[flake8-django] DJ001 is not raised for fields inherited from text fields"
type: issue
state: closed
author: sshishov
labels:
  - type-inference
assignees: []
created_at: 2023-10-04T06:15:06Z
updated_at: 2024-10-24T14:34:09Z
url: https://github.com/astral-sh/ruff/issues/7803
synced_at: 2026-01-07T13:12:15-06:00
---

# [flake8-django] DJ001 is not raised for fields inherited from text fields

---

_Issue opened by @sshishov on 2023-10-04 06:15_

In our project we are using custom fields with some mixings to extend the functionality of original fields
```python
class MyCharField(MyMixing, models.CharField):
    def __init__(self, *args, **kwargs):
        super().__init__(*args, *kwargs)
        <additional-pre-processing>


class MyModel(models.Model):
    my_field = MyCharField(max_length=100, null=True, blank=True)  # <-- the issue should be raised here, but it is not
```

Even easier example is not raising, like this:
```python
class MyCharField(models.CharField):
    pass


class MyModel(models.Model):
    my_field = MyCharField(max_length=100, null=True, blank=True)  # <-- the issue should be raised here, but it is not
```

---

_Comment by @charliermarsh on 2023-10-04 14:45_

Thanks for filing. Ruff doesn't support cross-file analysis right now, so the best we could do here is flag cases like this in which the field is defined in the same file as the model. My guess is that wouldn't be very helpful for your projects (I'm assuming these fields are defined in a central location, then imported into other files?), or would it?

---

_Label `question` added by @charliermarsh on 2023-10-04 14:45_

---

_Comment by @sshishov on 2023-10-04 16:16_

Hi @charliermarsh, the custom field files defined in separate module, and for some apps it can be even separate packages.

Is it possible just to look into what the parent for the current field and if it is `CharField` or `TextField` - apply the same rule? I do not know how everything is working internally...

Just a question, how do you understand that it is `django.db.models.CharField`, analyzing the import and Module and then flag it?

---

_Label `question` removed by @charliermarsh on 2023-10-08 18:19_

---

_Label `multifile-analysis` added by @charliermarsh on 2023-10-08 18:19_

---

_Comment by @charliermarsh on 2023-10-08 18:19_

It would be possible to do that, but not possible to support those kinds of checks across files unfortunately (yet).

> Just a question, how do you understand that it is `django.db.models.CharField`, analyzing the import and Module and then flag it?

Yeah, we track all the imports in a given file. Then, when we see something like `models.CharField`, we're able to trace that back to `from django.db import models`, and determine that it's `django.db.models.CharField`.

I'm going to close for now as unsupported but tag this with a multi-file label so that we know to come back to it in the future.


---

_Closed by @charliermarsh on 2023-10-08 18:19_

---

_Comment by @sshishov on 2023-10-08 21:14_

Thank you very much @charliermarsh for the explanation 👍 

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:34_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:34_

---
