```yaml
number: 2212
title: allow BLE001 if traceback is generated?
type: issue
state: closed
author: spaceone
labels:
  - bug
assignees: []
created_at: 2023-01-26T18:20:35Z
updated_at: 2024-06-10T17:31:15Z
url: https://github.com/astral-sh/ruff/issues/2212
synced_at: 2026-01-10T11:09:45Z
```

# allow BLE001 if traceback is generated?

---

_Issue opened by @spaceone on 2023-01-26 18:20_

`BLE001` allows
```python
try:
        foo
except Exception:
        raise
```
but disallows:
```python
try:
        foo
except Exception:
        print(traceback.format_exc())
        logger.log(traceback.format_exc())
        logger.exception('something is broken')
```

I think generating a traceback is an indication of handling the exception properly.

---

_Comment by @charliermarsh on 2023-01-26 21:18_

I agree.

---

_Label `bug` added by @charliermarsh on 2023-01-26 21:18_

---

_Comment by @charliermarsh on 2023-01-26 21:19_

This is also ok:

```py
try:
        foo
except Exception as e:
        raise e
```

---

_Closed by @charliermarsh on 2023-01-26 22:05_

---

_Comment by @ericbuehl on 2024-06-10 17:14_

Would it be appropriate to make this new logic optional so that ruff can be consistent with Pylint's [broad-exception-caught](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/broad-exception-caught.html)? Or perhaps the specific "error"/"exception" allowed calls could be configurable?  Happy to submit a PR if others agree

---
