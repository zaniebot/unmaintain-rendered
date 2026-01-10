```yaml
number: 7772
title: C414 removes comment
type: issue
state: closed
author: JelleZijlstra
labels:
  - bug
assignees: []
created_at: 2023-10-03T03:47:55Z
updated_at: 2023-10-03T14:17:23Z
url: https://github.com/astral-sh/ruff/issues/7772
synced_at: 2026-01-10T11:09:50Z
```

# C414 removes comment

---

_Issue opened by @JelleZijlstra on 2023-10-03 03:47_

```
$ cat xxx.py 
xxxxxxxxxxx_xxxxx_xxxxx = sorted(
    list(x_xxxx_xxxxxxxxxxx_xxxxx.xxxx()),
    # xxxxxxxxxxx xxxxx xxxx xxx xx Nxxx, xxx xxxxxx3 xxxxxxxxx xx
    # xx xxxx xxxxxxx xxxx xxx xxxxxxxx Nxxx
    key=lambda xxxxx: xxxxx or "",
)
$ python -m ruff --version
ruff 0.0.292
$ python -m ruff check --isolated --select=C414 --fix --diff xxx.py 
--- xxx.py
+++ xxx.py
@@ -1,6 +1,3 @@
 xxxxxxxxxxx_xxxxx_xxxxx = sorted(
-    list(x_xxxx_xxxxxxxxxxx_xxxxx.xxxx()),
-    # xxxxxxxxxxx xxxxx xxxx xxx xx Nxxx, xxx xxxxxx3 xxxxxxxxx xx
-    # xx xxxx xxxxxxx xxxx xxx xxxxxxxx Nxxx
-    key=lambda xxxxx: xxxxx or "",
+    x_xxxx_xxxxxxxxxxx_xxxxx.xxxx(), key=lambda xxxxx: xxxxx or "",
 )

Would fix 1 error.
```

---

_Label `bug` added by @charliermarsh on 2023-10-03 04:07_

---

_Comment by @charliermarsh on 2023-10-03 04:07_

Thanks. We handle these fixes manually with LibCST. We should be able to preserve the comments.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-03 04:22_

---

_Closed by @charliermarsh on 2023-10-03 04:36_

---

_Comment by @JelleZijlstra on 2023-10-03 04:38_

Thanks for the quick fixes on both of my issues!

---

_Comment by @charliermarsh on 2023-10-03 14:17_

Thanks for filing, appreciate it!

---
