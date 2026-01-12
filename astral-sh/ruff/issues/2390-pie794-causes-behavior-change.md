```yaml
number: 2390
title: PIE794 causes behavior change
type: issue
state: closed
author: spaceone
labels:
  - fixes
assignees: []
created_at: 2023-01-31T12:26:07Z
updated_at: 2023-06-08T16:58:31Z
url: https://github.com/astral-sh/ruff/issues/2390
synced_at: 2026-01-12T15:54:42Z
```

# PIE794 causes behavior change

---

_@spaceone_

```
$ ruff --select PIE794 --fix --diff foo.py
--- foo.py
+++ foo.py
@@ -1,3 +1,2 @@
 class foo(object):
     bar = 1 
-    bar = 2

Would fix 1 error.
```

              Yeah that's a reasonable argument. Okay, I'll do another pass on this.

_Originally posted by @charliermarsh in https://github.com/charliermarsh/ruff/issues/1787#issuecomment-1379609301_
            

---

_Label `autofix` added by @charliermarsh on 2023-01-31 12:29_

---

_Comment by @charliermarsh on 2023-06-08 16:58_

Closing as we made this a "suggested" fix, so in the future it'll only run with `--fix-unsafe`.

---

_Closed by @charliermarsh on 2023-06-08 16:58_

---
