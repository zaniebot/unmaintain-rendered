---
number: 1275
title: "Editable installs move around (relative to `pip-compile`)"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-02-11T23:16:00Z
updated_at: 2024-02-12T02:26:42Z
url: https://github.com/astral-sh/uv/issues/1275
synced_at: 2026-01-07T13:12:16-06:00
---

# Editable installs move around (relative to `pip-compile`)

---

_Issue opened by @charliermarsh on 2024-02-11 23:16_

```diff
diff --git a/requirements-dev.lock b/requirements-dev.lock
index 5879576..8353ebd 100644
--- a/requirements-dev.lock
+++ b/requirements-dev.lock
@@ -7,8 +7,6 @@
 #   all-features: false
 #   with-sources: false

--e file:.
-    # from workspace
 certifi==2023.5.7
     # via requests
 charset-normalizer==3.1.0
@@ -68,6 +66,7 @@ regex==2023.5.5
     # via mkdocs-material
 requests==2.30.0
     # via mkdocs-material
+-e file:.
 six==1.16.0
     # via python-dateutil
 urllib3==2.0.2
```

---

_Label `bug` added by @charliermarsh on 2024-02-11 23:16_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-12 00:49_

---

_Referenced in [astral-sh/uv#1278](../../astral-sh/uv/pulls/1278.md) on 2024-02-12 02:00_

---

_Closed by @charliermarsh on 2024-02-12 02:26_

---
