```yaml
number: 2814
title: TRY201 wrongly complains about raising from None
type: issue
state: closed
author: adamtheturtle
labels:
  - bug
assignees: []
created_at: 2023-02-12T14:55:46Z
updated_at: 2023-02-12T16:48:15Z
url: https://github.com/astral-sh/ruff/issues/2814
synced_at: 2026-01-12T15:54:43Z
```

# TRY201 wrongly complains about raising from None

---

_@adamtheturtle_

Take the following example:

```python
try:
    raise Exception("We want to hide this error message")
except Exception:
    try:
        raise Exception("We want to show this")
    except Exception as exc:
        raise exc from None
```

`ruff` with `TRY201` selected tells us:

```
TRY201 Use `raise` without specifying exception name
```

However, we cannot do `raise from None`.

See https://peps.python.org/pep-0409/#proposal for context of this syntax.

---

_Label `bug` added by @charliermarsh on 2023-02-12 15:03_

---

_Closed by @charliermarsh on 2023-02-12 16:48_

---

_Closed by @charliermarsh on 2023-02-12 16:48_

---
