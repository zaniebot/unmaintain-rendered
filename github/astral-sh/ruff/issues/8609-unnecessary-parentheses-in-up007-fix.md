---
number: 8609
title: Unnecessary parentheses in UP007 fix
type: issue
state: closed
author: hauntsaninja
labels:
  - bug
  - fixes
assignees: []
created_at: 2023-11-10T23:29:46Z
updated_at: 2023-11-11T00:15:10Z
url: https://github.com/astral-sh/ruff/issues/8609
synced_at: 2026-01-07T13:12:15-06:00
---

# Unnecessary parentheses in UP007 fix

---

_Issue opened by @hauntsaninja on 2023-11-10 23:29_

```
λ cat x.py
from typing import Union

def foo(x: Union[int, str, bytes]):
    pass

λ ruff check --select=UP007 --fix --unsafe-fixes --diff --target-version py311 x.py
--- x.py
+++ x.py
@@ -1,4 +1,4 @@
 from typing import Union
 
-def foo(x: Union[int, str, bytes]):
+def foo(x: int | (str | bytes)):
     pass

Would fix 1 error.
```

---

_Label `bug` added by @charliermarsh on 2023-11-10 23:43_

---

_Label `autofix` added by @charliermarsh on 2023-11-10 23:43_

---

_Referenced in [astral-sh/ruff#8610](../../astral-sh/ruff/pulls/8610.md) on 2023-11-10 23:50_

---

_Closed by @charliermarsh on 2023-11-11 00:15_

---
