```yaml
number: 2427
title: SIM autofix should check if bool is builtin
type: issue
state: closed
author: spaceone
labels:
  - fixes
assignees: []
created_at: 2023-01-31T23:21:27Z
updated_at: 2023-02-01T00:03:47Z
url: https://github.com/astral-sh/ruff/issues/2427
synced_at: 2026-01-12T15:54:42Z
```

# SIM autofix should check if bool is builtin

---

_@spaceone_

This example should not be rewritten:
```
$ ruff --select SIM --fix --diff foo.py 
--- foo.py
+++ foo.py
@@ -1,4 +1,4 @@
 def bool(a):
     return False
 
-a = True if b else False
+a = bool(b)
```

---

_Comment by @charliermarsh on 2023-01-31 23:24_

We can do this! `checker.is_builtin`.

---

_Comment by @charliermarsh on 2023-01-31 23:25_

Do you want to fix it?

---

_Label `autofix` added by @charliermarsh on 2023-01-31 23:25_

---

_Closed by @charliermarsh on 2023-02-01 00:03_

---
