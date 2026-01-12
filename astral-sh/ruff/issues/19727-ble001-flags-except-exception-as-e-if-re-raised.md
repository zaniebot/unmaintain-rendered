```yaml
number: 19727
title: "BLE001 flags `except Exception as e` if re-raised `from None`"
type: issue
state: closed
author: injust
labels:
  - bug
  - rule
assignees: []
created_at: 2025-08-04T07:03:21Z
updated_at: 2025-08-13T17:01:48Z
url: https://github.com/astral-sh/ruff/issues/19727
synced_at: 2026-01-12T15:54:57Z
```

# BLE001 flags `except Exception as e` if re-raised `from None`

---

_@injust_

### Summary

This doesn't trigger BLE001:

```python
try:
    pass
except Exception as e:
    raise e
```

but this does:

```python
try:
    pass
except Exception as e:
    raise e from None
```

https://play.ruff.rs/a79e86c8-79e4-4798-bb72-fee7e5908ea9

Related: https://github.com/astral-sh/ruff/issues/2212

cc @charliermarsh

### Version

ruff 0.12.7 (c5ac99889 2025-07-29)

---

_Comment by @ntBre on 2025-08-05 13:01_

I think this makes sense. It looks like we currently only support `raise-from` if the target matches the exception name, as in:

```py
try:
    ...
except Exception as e:
    raise ValueError from e
```

from https://github.com/astral-sh/ruff/pull/11131.

---

_Label `rule` added by @ntBre on 2025-08-05 13:01_

---

_Label `bug` added by @ntBre on 2025-08-05 13:01_

---

_Closed by @ntBre on 2025-08-13 17:01_

---
