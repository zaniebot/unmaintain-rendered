```yaml
number: 11003
title: "Respect `--no-sources` for `uv pip install` workspace discovery"
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/uv-pip-install-source-dont-discover-workspaces
created_at: 2025-01-27T21:22:05Z
updated_at: 2025-01-28T00:19:03Z
url: https://github.com/astral-sh/uv/pull/11003
synced_at: 2026-01-12T16:09:37Z
```

# Respect `--no-sources` for `uv pip install` workspace discovery

---

_@konstin_

Don't discover workspace members with `uv pip install` when `--no-sources` is given.

The problem did only occur with `uv pip install`, not with `uv pip compile`.

Fixes #10999

---

_Label `bug` added by @konstin on 2025-01-27 21:22_

---

_Review requested from @charliermarsh by @konstin on 2025-01-27 21:22_

---

_@charliermarsh approved on 2025-01-27 23:07_

---

_Merged by @konstin on 2025-01-27 23:10_

---

_Closed by @konstin on 2025-01-27 23:10_

---

_Branch deleted on 2025-01-27 23:10_

---

_Comment by @potiuk on 2025-01-28 00:19_

Nice! You are amazing!

---
