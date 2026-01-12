```yaml
number: 4102
title: Ignore dependencies with unsupported Python versions during universal lock
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
created_at: 2024-06-06T14:44:23Z
updated_at: 2024-06-26T12:55:13Z
url: https://github.com/astral-sh/uv/issues/4102
synced_at: 2026-01-12T15:58:48Z
```

# Ignore dependencies with unsupported Python versions during universal lock

---

_@charliermarsh_

If the user has `requires-python = ">= 3.7"`, we should be able to prune dependencies with (e.g.) `flask ; python_version == '3.3'`.


---

_Label `bug` added by @charliermarsh on 2024-06-06 14:44_

---

_Label `preview` added by @charliermarsh on 2024-06-06 14:44_

---

_Assigned to @BurntSushi by @BurntSushi on 2024-06-06 14:44_

---

_Comment by @BurntSushi on 2024-06-26 12:55_

Closed by https://github.com/astral-sh/uv/pull/4273

---

_Closed by @BurntSushi on 2024-06-26 12:55_

---
