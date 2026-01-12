```yaml
number: 11524
title: Respect verbatim executable name in uvx
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/x
created_at: 2025-02-14T20:47:02Z
updated_at: 2025-02-14T21:25:18Z
url: https://github.com/astral-sh/uv/pull/11524
synced_at: 2026-01-12T16:09:53Z
```

# Respect verbatim executable name in uvx

---

_@charliermarsh_

## Summary

If the user provides a PEP 508 requirement (e.g., `uvx change_wheel_version`), then we should us that verbatim for the executable, rather than normalizing the package name.

Closes https://github.com/astral-sh/uv/issues/11521.


---

_Label `bug` added by @charliermarsh on 2025-02-14 20:47_

---

_Review requested from @zanieb by @charliermarsh on 2025-02-14 20:47_

---

_@zanieb reviewed on 2025-02-14 20:49_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_run.rs`:1890 on 2025-02-14 20:49_

This is what you're trying to fix, right?

---

_@charliermarsh reviewed on 2025-02-14 20:52_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/tool_run.rs`:1890 on 2025-02-14 20:52_

No, but this _is_ wrong, I just forgot to update the snapshot. The thing I'm trying to fix is the first snapshot.

---

_@zanieb approved on 2025-02-14 20:57_

---

_Merged by @charliermarsh on 2025-02-14 21:25_

---

_Closed by @charliermarsh on 2025-02-14 21:25_

---

_Branch deleted on 2025-02-14 21:25_

---
