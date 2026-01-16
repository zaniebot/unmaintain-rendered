```yaml
number: 17514
title: "Remove now redundant `env_remove` calls"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/clear-env-removals-in-tests
created_at: 2026-01-16T12:13:42Z
updated_at: 2026-01-16T15:17:52Z
url: https://github.com/astral-sh/uv/pull/17514
synced_at: 2026-01-16T15:58:35Z
```

# Remove now redundant `env_remove` calls

---

_@konstin_

Follow-up to https://github.com/astral-sh/uv/pull/14080


---

_Label `internal` added by @konstin on 2026-01-16 12:13_

---

_@konstin reviewed on 2026-01-16 12:14_

---

_Review comment by @konstin on `crates/uv/tests/it/python_find.rs`:624 on 2026-01-16 12:14_

@zanieb `python_find` doesn't activate the venv, so it seems like this and the branch above are testing the same thing

---

_@zanieb reviewed on 2026-01-16 13:10_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_find.rs`:624 on 2026-01-16 13:10_

Hm makes sense, so we should drop it?

---

_@zanieb approved on 2026-01-16 13:10_

---

_Merged by @konstin on 2026-01-16 15:17_

---

_Closed by @konstin on 2026-01-16 15:17_

---

_Branch deleted on 2026-01-16 15:17_

---
