---
number: 13887
title: "[flake8-django] DJ001 is not raised for fields which has unique=True"
type: issue
state: closed
author: kiddten
labels:
  - question
assignees: []
created_at: 2024-10-23T11:49:09Z
updated_at: 2024-11-20T12:01:41Z
url: https://github.com/astral-sh/ruff/issues/13887
synced_at: 2026-01-10T01:22:54Z
---

# [flake8-django] DJ001 is not raised for fields which has unique=True

---

_Issue opened by @kiddten on 2024-10-23 11:49_

```
class Product(models.Model):
    test = models.CharField(null=True, blank=True, max_length=255)
```
is correct

> DJ001 Avoid using `null=True` on string-based fields such as `CharField`

but

```
class Product(models.Model):
    test = models.CharField(null=True, blank=True, max_length=255, unique=True)
```

> All checks passed!

gives no warning



ruff version 0.7.0

---

_Comment by @kiddten on 2024-10-23 11:54_

Hmm looks like this is expected behaviour https://github.com/astral-sh/ruff/blob/4d109514d608b45bc11a5ecf3e022112461cb571/crates/ruff_linter/src/rules/flake8_django/rules/nullable_model_string_field.rs#L105

---

_Comment by @UnknownPlatypus on 2024-10-23 12:10_

See https://docs.djangoproject.com/en/5.1/ref/models/fields/#null for the rationale. This is indeed expected behavior

---

_Label `question` added by @MichaReiser on 2024-10-23 12:13_

---

_Closed by @MichaReiser on 2024-11-20 12:01_

---
