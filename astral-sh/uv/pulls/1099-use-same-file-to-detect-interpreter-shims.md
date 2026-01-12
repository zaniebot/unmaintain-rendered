```yaml
number: 1099
title: "Use `same-file` to detect interpreter shims"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/same-file
created_at: 2024-01-25T16:43:42Z
updated_at: 2024-01-25T17:27:50Z
url: https://github.com/astral-sh/uv/pull/1099
synced_at: 2026-01-12T16:04:25Z
```

# Use `same-file` to detect interpreter shims

---

_@charliermarsh_

Our existing detection doesn't work on Windows, because we canoncalize the interpreter path but not `info.sys_executable`, so the former includes the UNC prefix, etc. This is cross-platform and gets at the intent of the check.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-01-25 16:43_

---

_Label `bug` added by @charliermarsh on 2024-01-25 16:43_

---

_@BurntSushi approved on 2024-01-25 16:45_

:D 

---

_Merged by @charliermarsh on 2024-01-25 17:27_

---

_Closed by @charliermarsh on 2024-01-25 17:27_

---

_Branch deleted on 2024-01-25 17:27_

---
