```yaml
number: 17514
title: "Remove now redundant `env_remove` calls"
type: pull_request
state: open
author: konstin
labels:
  - internal
assignees: []
base: main
head: konsti/clear-env-removals-in-tests
created_at: 2026-01-16T12:13:42Z
updated_at: 2026-01-16T12:14:32Z
url: https://github.com/astral-sh/uv/pull/17514
synced_at: 2026-01-16T12:56:44Z
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
