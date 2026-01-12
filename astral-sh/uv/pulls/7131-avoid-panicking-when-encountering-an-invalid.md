```yaml
number: 7131
title: "Avoid panicking when encountering an invalid Python version during `uv python list`"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-panic-version
created_at: 2024-09-06T17:19:32Z
updated_at: 2024-09-06T23:23:17Z
url: https://github.com/astral-sh/uv/pull/7131
synced_at: 2026-01-12T16:07:42Z
```

# Avoid panicking when encountering an invalid Python version during `uv python list`

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/7129

Not entirely sure about the best approach yet.

---

_Label `bug` added by @zanieb on 2024-09-06 17:19_

---

_Marked ready for review by @zanieb on 2024-09-06 18:22_

---

_@charliermarsh approved on 2024-09-06 23:16_

---

_Comment by @charliermarsh on 2024-09-06 23:17_

I also think we should consider removing the `> 3.6, < 4` guard in `PythonVersion`. That should probably be enforced somewhere else. It seems reasonable to show Python 3.6 versions in `uv python list`, right?

---

_Merged by @charliermarsh on 2024-09-06 23:23_

---

_Closed by @charliermarsh on 2024-09-06 23:23_

---

_Branch deleted on 2024-09-06 23:23_

---
