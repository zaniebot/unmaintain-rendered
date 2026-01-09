---
number: 7478
title: "Extend `bad-dunder-method-name` to permit `__html__`"
type: issue
state: closed
author: jaap3
labels:
  - accepted
assignees: []
created_at: 2023-09-18T07:37:21Z
updated_at: 2023-09-18T15:16:24Z
url: https://github.com/astral-sh/ruff/issues/7478
synced_at: 2026-01-07T13:12:15-06:00
---

# Extend `bad-dunder-method-name` to permit `__html__`

---

_Issue opened by @jaap3 on 2023-09-18 07:37_

The `__html__` dunder method is used by [Jinja2](https://jinja.palletsprojects.com/en/3.1.x/templates/#jinja-filters.escape), [Django](https://docs.djangoproject.com/en/4.2/_modules/django/utils/safestring/) and other frameworks (?) to return the "safe" html representation of an object.

There doesn't seem to be an equivalent of [pylint's good-dunder-names](https://pylint.readthedocs.io/en/stable/user_guide/configuration/all-options.html#good-dunder-names) in ruff. Would it be acceptable to add `__html__` to the allow list?


---

_Comment by @jaap3 on 2023-09-18 07:43_

I'm willing to make a PR if this is accepted

---

_Comment by @charliermarsh on 2023-09-18 13:08_

Yes that seems reasonable. PR appreciated!

---

_Label `accepted` added by @charliermarsh on 2023-09-18 13:08_

---

_Referenced in [astral-sh/ruff#7492](../../astral-sh/ruff/pulls/7492.md) on 2023-09-18 15:04_

---

_Closed by @dhruvmanila on 2023-09-18 15:16_

---
