```yaml
number: 3256
title: "PLE1205: false positive for names arguments"
type: issue
state: closed
author: spaceone
labels:
  - bug
assignees: []
created_at: 2023-02-27T18:37:31Z
updated_at: 2023-02-27T23:11:15Z
url: https://github.com/astral-sh/ruff/issues/3256
synced_at: 2026-01-10T11:09:46Z
```

# PLE1205: false positive for names arguments

---

_Issue opened by @spaceone on 2023-02-27 18:37_

```python
>>> import logging
>>> logging.error("%(objects)d modifications: %(modifications)d errors: %(errors)d", {"objects": 1, "modifications": 1, "errors": 1})
ERROR:root:1 modifications: 1 errors: 1
```
is detected as PLE1205 logging-too-many-args while this is actual the way how these arguments must be passed. giving them as keyword arguments raises:

```python
>>> import logging
>>> logging.error("%(objects)d modifications: %(modifications)d errors: %(errors)d", **{"objects": 1, "modifications": 1, "errors": 1})
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/lib/python3.7/logging/__init__.py", line 1961, in error
    root.error(msg, *args, **kwargs)
  File "/usr/lib/python3.7/logging/__init__.py", line 1412, in error
    self._log(ERROR, msg, args, **kwargs)
TypeError: _log() got an unexpected keyword argument 'objects'
```

---

_Renamed from "PLE1206: false positive for names arguments" to "PLE1205: false positive for names arguments" by @spaceone on 2023-02-27 18:38_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-27 21:38_

---

_Label `bug` added by @charliermarsh on 2023-02-27 21:52_

---

_Closed by @charliermarsh on 2023-02-27 23:11_

---
