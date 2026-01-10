```yaml
number: 1043
title: Ruff incorrectly reports on redefined imports with explicit re-exports
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2022-12-04T19:28:03Z
updated_at: 2022-12-12T00:49:19Z
url: https://github.com/astral-sh/ruff/issues/1043
synced_at: 2026-01-10T12:06:15Z
```

# Ruff incorrectly reports on redefined imports with explicit re-exports

---

_Issue opened by @charliermarsh on 2022-12-04 19:28_

Given this snippet:

```py
import os
import os as os
```

Ruff reports an unused import on line 2, although that's an explicit re-export:

```py
∴ cargo run foo.py
    Finished dev [unoptimized + debuginfo] target(s) in 0.14s
     Running `target/debug/ruff foo.py`
Found 1 error(s).
foo.py:2:8: F401 `os` imported but unused
```

---

_Label `bug` added by @charliermarsh on 2022-12-04 19:28_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-04 19:28_

---

_Comment by @charliermarsh on 2022-12-04 19:47_

It's easy to fix:

```py
import os
import os as os
```

But harder to fix:

```py
import os as os
import os
```

Fixing the latter requires that we can detect redefinition (see: #996).

---

_Comment by @charliermarsh on 2022-12-12 00:49_

Oh, this works now.

---

_Closed by @charliermarsh on 2022-12-12 00:49_

---
