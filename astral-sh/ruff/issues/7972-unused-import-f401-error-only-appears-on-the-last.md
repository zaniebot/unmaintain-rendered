```yaml
number: 7972
title: "`unused-import` (`F401`) - error only appears on the last unused import"
type: issue
state: closed
author: DetachHead
labels: []
assignees: []
created_at: 2023-10-16T03:26:23Z
updated_at: 2023-10-16T03:30:29Z
url: https://github.com/astral-sh/ruff/issues/7972
synced_at: 2026-01-10T11:09:50Z
```

# `unused-import` (`F401`) - error only appears on the last unused import

---

_Issue opened by @DetachHead on 2023-10-16 03:26_

```py
import foo.bar # no error
import foo.baz # no error
import foo.qux # error
```
https://play.ruff.rs/2f02d4f5-bed4-4cf2-9333-ad28a600c603

---

_Comment by @charliermarsh on 2023-10-16 03:30_

I believe this is a duplicate of https://github.com/astral-sh/ruff/issues/60.

---

_Closed by @charliermarsh on 2023-10-16 03:30_

---
