---
number: 9304
title: Incorrect ANN201 autofix
type: issue
state: closed
author: hauntsaninja
labels:
  - bug
assignees: []
created_at: 2023-12-29T08:20:00Z
updated_at: 2024-01-03T03:59:13Z
url: https://github.com/astral-sh/ruff/issues/9304
synced_at: 2026-01-10T01:22:49Z
---

# Incorrect ANN201 autofix

---

_Issue opened by @hauntsaninja on 2023-12-29 08:20_

Given:
```
from somewhere import get_cfg

def lookup_cfg(cfg_description):
    cfg = get_cfg(cfg_description)
    if cfg is not None:
        return cfg
    raise AttributeError(f"No cfg found matching {cfg_description}")
```

ANN201 suggests the following incorrect fix:
```
λ ruff check --select=ANN2 x.py --fix --unsafe-fixes --diff
--- x.py
+++ x.py
@@ -1,6 +1,7 @@
 from somewhere import get_cfg
+from typing import Never
 
-def lookup_cfg(cfg_description):
+def lookup_cfg(cfg_description) -> Never:
     cfg = get_cfg(cfg_description)
     if cfg is not None:
         return cfg

Would fix 1 error.
```

---

_Comment by @hauntsaninja on 2023-12-29 08:27_

Presumably relates to https://github.com/astral-sh/ruff/pull/9206

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-29 12:41_

---

_Label `bug` added by @charliermarsh on 2023-12-29 12:42_

---

_Referenced in [astral-sh/ruff#9310](../../astral-sh/ruff/pulls/9310.md) on 2023-12-29 16:19_

---

_Closed by @charliermarsh on 2023-12-29 16:46_

---

_Comment by @hauntsaninja on 2023-12-30 09:08_

Thanks! :-)

---

_Comment by @hauntsaninja on 2024-01-03 01:04_

I think the behaviour still isn't quite right:
```
λ cat xx.py
def foo(a):
    if a > 5:
        raise ValueError("oops")
    else:
        print("nope")
λ ruff --version
ruff 0.1.11
λ ruff check xx.py --select=ANN2 --fix --unsafe-fixes --diff
--- xx.py
+++ xx.py
@@ -1,4 +1,5 @@
-def foo(a):
+from typing import Never
+def foo(a) -> Never:
     if a > 5:
         raise ValueError("oops")
     else:

Would fix 1 error.
```

---

_Comment by @charliermarsh on 2024-01-03 01:27_

🤦 One more try...

---

_Reopened by @charliermarsh on 2024-01-03 01:27_

---

_Referenced in [astral-sh/ruff#9377](../../astral-sh/ruff/pulls/9377.md) on 2024-01-03 03:16_

---

_Closed by @charliermarsh on 2024-01-03 03:59_

---
