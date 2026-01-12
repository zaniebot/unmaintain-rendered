```yaml
number: 15831
title: "Error when `pyproject.toml` target does not exist for dependency groups"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/discover
created_at: 2025-09-14T01:55:26Z
updated_at: 2025-09-14T13:49:36Z
url: https://github.com/astral-sh/uv/pull/15831
synced_at: 2026-01-12T16:11:57Z
```

# Error when `pyproject.toml` target does not exist for dependency groups

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/15789.


---

_Review requested from @konstin by @charliermarsh on 2025-09-14 01:57_

---

_Review requested from @Gankra by @charliermarsh on 2025-09-14 01:57_

---

_Label `error messages` added by @charliermarsh on 2025-09-14 01:57_

---

_Marked ready for review by @charliermarsh on 2025-09-14 01:57_

---

_@charliermarsh reviewed on 2025-09-14 01:58_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_compile.rs`:16197 on 2025-09-14 01:58_

For context, at present, if the `pyproject.toml` doesn't exist but the parent `pyproject.toml` is valid (so discovery succeeds), you get:
```
error: The dependency group 'foo' was not found in the project: `does/not/exist/pyproject.toml
```

---

_Review comment by @konstin on `crates/uv/src/commands/pip/operations.rs`:221 on 2025-09-14 11:04_

@zanieb @charliermarsh We should have a consistent style on whether we place backticks after colons or not.

---

_@konstin reviewed on 2025-09-14 11:05_

---

_Review requested from @konstin by @charliermarsh on 2025-09-14 13:23_

---

_@charliermarsh reviewed on 2025-09-14 13:24_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip/operations.rs`:221 on 2025-09-14 13:24_

Going to roll this back to simplify the change

---

_Merged by @charliermarsh on 2025-09-14 13:49_

---

_Closed by @charliermarsh on 2025-09-14 13:49_

---

_Branch deleted on 2025-09-14 13:49_

---
