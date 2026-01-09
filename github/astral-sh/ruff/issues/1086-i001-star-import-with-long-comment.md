---
number: 1086
title: "I001: Star import with long comment"
type: issue
state: closed
author: UnknownPlatypus
labels: []
assignees: []
created_at: 2022-12-05T23:08:07Z
updated_at: 2022-12-05T23:39:17Z
url: https://github.com/astral-sh/ruff/issues/1086
synced_at: 2026-01-07T13:12:14-06:00
---

# I001: Star import with long comment

---

_Issue opened by @UnknownPlatypus on 2022-12-05 23:08_

A star import with a long comment is formatted into multiple lines which creates invalid python code.
See the following exemple:

```diff
-from .subscription import * # type: ignore  # some very long comment explaining why this needs a type ignore
+from .subscription import ( # type: ignore  # some very long comment explaining why this needs a type ignore
+    *,
+)
```

---

_Renamed from "IOO1: Star import with long comment" to "I001: Star import with long comment" by @charliermarsh on 2022-12-05 23:21_

---

_Referenced in [astral-sh/ruff#1089](../../astral-sh/ruff/pulls/1089.md) on 2022-12-05 23:39_

---

_Closed by @charliermarsh on 2022-12-05 23:39_

---

_Referenced in [astral-sh/ruff#7904](../../astral-sh/ruff/issues/7904.md) on 2023-10-11 00:07_

---
