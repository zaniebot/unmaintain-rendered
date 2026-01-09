---
number: 2580
title: UP034 introduces unnecessary whitespace-only line
type: issue
state: closed
author: spaceone
labels:
  - wontfix
assignees: []
created_at: 2023-02-05T10:48:00Z
updated_at: 2023-02-05T12:14:11Z
url: https://github.com/astral-sh/ruff/issues/2580
synced_at: 2026-01-07T13:12:14-06:00
---

# UP034 introduces unnecessary whitespace-only line

---

_Issue opened by @spaceone on 2023-02-05 10:48_

```
$ ruff --select UP034 --fix --diff foo.py
--- foo.py
+++ foo.py
@@ -1,6 +1,6 @@
 print(
-    (
+    
         "abc"
         "def"
-    )
+    
 )

Would fix 1 error.

```

→ the empty lines should not contain any whitespace

---

_Label `wontfix` added by @charliermarsh on 2023-02-05 12:13_

---

_Comment by @charliermarsh on 2023-02-05 12:14_

I think realistically we may be unlikely to fix this as part of the rule.

---

_Closed by @charliermarsh on 2023-02-05 12:14_

---
