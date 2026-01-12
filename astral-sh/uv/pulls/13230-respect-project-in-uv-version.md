```yaml
number: 13230
title: "Respect `--project` in `uv version`"
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/use-project-in-uv-version
created_at: 2025-04-30T13:58:40Z
updated_at: 2025-05-04T12:27:13Z
url: https://github.com/astral-sh/uv/pull/13230
synced_at: 2026-01-12T16:10:36Z
```

# Respect `--project` in `uv version`

---

_@konstin_

Previously, we were using the wrong `Workspace` discovery and would report the version of the workspace root, which would iterate up from the `--project` directory and return the workspace root (with or without a project in the root). Instead, we need `ProjectWorkspace` discovery that returns the closest project.

This fixes `uv version --project <path>` where `<path>` belongs to a workspace member.

Fixes #13213

---

_Label `bug` added by @konstin on 2025-04-30 13:58_

---

_Review requested from @Gankra by @konstin on 2025-04-30 13:58_

---

_Assigned to @Gankra by @konstin on 2025-04-30 13:58_

---

_@zanieb approved on 2025-04-30 14:01_

---

_Comment by @zanieb on 2025-04-30 14:01_

Do we need coverage for mutating operations too?

---

_@Gankra approved on 2025-04-30 14:04_

argh i knew i implemented and test this, but i missed this subtlety!

---

_Merged by @zanieb on 2025-04-30 16:11_

---

_Closed by @zanieb on 2025-04-30 16:11_

---

_Branch deleted on 2025-04-30 16:11_

---
