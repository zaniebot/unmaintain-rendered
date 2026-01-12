```yaml
number: 1243
title: "Arithmetic between `date` and `timedelta` results in `Unknown`, not `date`"
type: issue
state: closed
author: udbhav1
labels: []
assignees: []
created_at: 2025-09-23T19:51:09Z
updated_at: 2025-09-23T19:57:02Z
url: https://github.com/astral-sh/ty/issues/1243
synced_at: 2026-01-12T15:54:24Z
```

# Arithmetic between `date` and `timedelta` results in `Unknown`, not `date`

---

_@udbhav1_

### Summary

Doing arithmetic between a `date` and a `timedelta` should result in a `date`. Mypy correctly infers that, but `ty` thinks it's `Unknown`.

Ran into this locally, but my setup is somewhat complex so I just reproduced it in the playground:

```python
from datetime import date, timedelta
from typing import reveal_type

a = date(2025, 9, 7)
b = timedelta(days=5)
c = a + b
d = a - b

reveal_type(a) # date
reveal_type(b) # timedelta
reveal_type(c) # Unknown
reveal_type(d) # Unknown
```

https://play.ty.dev/99ff79fd-20b6-42a7-9a9b-cce0c095d46b

### Version

_No response_

---

_Comment by @AlexWaygood on 2025-09-23 19:55_

Thanks for the report! This is because `datetime.__add__` and the relevant overload of `datetime.__sub__` are both annotated as `Self` in typeshed (<https://github.com/python/typeshed/blob/b0d926ee22cad61f3f7bfb544867e98a324da1ff/stdlib/datetime.pyi#L103>, <https://github.com/python/typeshed/blob/b0d926ee22cad61f3f7bfb544867e98a324da1ff/stdlib/datetime.pyi#L110>), and we do not fully support `Self` yet

---

_Closed by @AlexWaygood on 2025-09-23 19:55_

---
