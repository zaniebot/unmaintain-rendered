```yaml
number: 7929
title: "Feature Request: Support isort literal ordering"
type: issue
state: closed
author: kkirsche
labels:
  - isort
assignees: []
created_at: 2023-10-12T14:42:01Z
updated_at: 2023-10-12T16:09:33Z
url: https://github.com/astral-sh/ruff/issues/7929
synced_at: 2026-01-10T11:09:50Z
```

# Feature Request: Support isort literal ordering

---

_Issue opened by @kkirsche on 2023-10-12 14:42_

This issue is to request support for isort's ordering action comments.

For example:

```python
# isort: unique-list
__all__ = ["b", "c", "a", "c"]
```

Would be rewritten to:

```python
# isort: unique-list
__all__ = [ "a", "b", "c"]
```

This code within isort is located here:
https://github.com/PyCQA/isort/blob/9e0ad3d9939e0649a77f4e04db23652f170808a6/isort/literal.py#L97

With the code styling comments supported by isort located here:
https://github.com/PyCQA/isort/blob/9e0ad3d9939e0649a77f4e04db23652f170808a6/isort/core.py#L18

This was originally requested within isort in the issue below and released as part of isort 5.3.0:
https://github.com/PyCQA/isort/issues/1358

Thank you for your time and consideration.

---

_Comment by @charliermarsh on 2023-10-12 16:09_

Thanks for the clear issue. I'm going to merge this into https://github.com/astral-sh/ruff/issues/1198 since we're tracking these collection comments there.

---

_Closed by @charliermarsh on 2023-10-12 16:09_

---

_Label `isort` added by @charliermarsh on 2023-10-12 16:09_

---
