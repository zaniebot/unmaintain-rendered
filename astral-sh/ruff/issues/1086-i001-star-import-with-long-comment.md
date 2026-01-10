```yaml
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
synced_at: 2026-01-10T12:06:16Z
```

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

_Closed by @charliermarsh on 2022-12-05 23:39_

---
