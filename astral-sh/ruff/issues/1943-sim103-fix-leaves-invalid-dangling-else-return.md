```yaml
number: 1943
title: "SIM103 fix leaves invalid dangling `else: return False`"
type: issue
state: closed
author: andersk
labels:
  - bug
assignees: []
created_at: 2023-01-18T02:42:57Z
updated_at: 2023-01-18T03:03:18Z
url: https://github.com/astral-sh/ruff/issues/1943
synced_at: 2026-01-10T11:09:44Z
```

# SIM103 fix leaves invalid dangling `else: return False`

---

_Issue opened by @andersk on 2023-01-18 02:42_

```diff
$ ruff --select=SIM103 --diff test.py
--- test.py
+++ test.py
@@ -1,7 +1,6 @@
 def f():
     if 1:
         return 2
-    elif 3 and 4:
-        return True
+    return (3 and 4)
     else:
         return False

Would fix 1 error(s).
```

Ruff forgot to delete the `else:` block, so the resulting code is invalid. Also, the extra parens are unnecessary.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-18 02:47_

---

_Label `bug` added by @charliermarsh on 2023-01-18 02:47_

---

_Closed by @charliermarsh on 2023-01-18 03:03_

---
