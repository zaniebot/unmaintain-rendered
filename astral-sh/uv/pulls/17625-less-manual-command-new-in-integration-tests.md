```yaml
number: 17625
title: "Less manual `Command::new` in integration tests"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/more-consistent-tests
created_at: 2026-01-20T15:01:44Z
updated_at: 2026-01-20T15:54:22Z
url: https://github.com/astral-sh/uv/pull/17625
synced_at: 2026-01-20T16:47:12Z
```

# Less manual `Command::new` in integration tests

---

_@konstin_

Improve test consistency a bit, with an eye towards environment variable usage.

Notably, use `TestContext::pip_uninstall` and `TestContext::pip_tree` over `Command::new`.

---

_Label `internal` added by @konstin on 2026-01-20 15:01_

---

_@zanieb approved on 2026-01-20 15:03_

---

_Merged by @konstin on 2026-01-20 15:54_

---

_Closed by @konstin on 2026-01-20 15:54_

---

_Branch deleted on 2026-01-20 15:54_

---
