```yaml
number: 3695
title: Support editables in lockfile
type: issue
state: closed
author: charliermarsh
labels:
  - preview
assignees: []
created_at: 2024-05-21T12:15:26Z
updated_at: 2024-05-22T13:06:20Z
url: https://github.com/astral-sh/uv/issues/3695
synced_at: 2026-01-12T15:58:45Z
```

# Support editables in lockfile

---

_@charliermarsh_

E.g., in `uv sync` and `uv lock`.

---

_Label `preview` added by @charliermarsh on 2024-05-21 12:15_

---

_Comment by @charliermarsh on 2024-05-21 12:15_

@BurntSushi - these aren't represented yet, right?

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-21 12:19_

---

_Comment by @BurntSushi on 2024-05-21 15:00_

I think I assumed that editables would be path dependencies, which are in the lock file data model. But other than that, no.

---

_Comment by @charliermarsh on 2024-05-21 15:00_

Yeah, they're directory dependencies, but they're installed differently.

---

_Closed by @charliermarsh on 2024-05-22 13:06_

---
