```yaml
number: 10775
title: Ignore lockfile in ecosystem tests
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/fix-ecosystem-tests-uv-lock
created_at: 2025-01-20T10:25:59Z
updated_at: 2025-01-20T10:34:47Z
url: https://github.com/astral-sh/uv/pull/10775
synced_at: 2026-01-10T11:45:09Z
```

# Ignore lockfile in ecosystem tests

---

_Pull request opened by @konstin on 2025-01-20 10:25_

The ecosystem tests can fail when there is a (gitignored) `uv.lock` in `ecosystem/<name>/uv.lock`.

Moving the snapshots means we're seeing the actual lock difference.

---

_Label `internal` added by @konstin on 2025-01-20 10:25_

---

_Merged by @konstin on 2025-01-20 10:34_

---

_Closed by @konstin on 2025-01-20 10:34_

---

_Branch deleted on 2025-01-20 10:34_

---
