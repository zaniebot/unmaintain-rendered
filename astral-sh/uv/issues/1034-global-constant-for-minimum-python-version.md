---
number: 1034
title: Global constant for minimum python version
type: issue
state: closed
author: konstin
labels:
  - internal
assignees: []
created_at: 2024-01-22T08:40:58Z
updated_at: 2024-06-27T11:26:16Z
url: https://github.com/astral-sh/uv/issues/1034
synced_at: 2026-01-10T01:23:05Z
---

# Global constant for minimum python version

---

_Issue opened by @konstin on 2024-01-22 08:40_

Multiple places across the codebase rely on knowing the minimum supported python version, we should introduce a global constant that's shared across crates. This will ensure the minimum is enforced consistently and will ease bumping the version later.

---

_Label `internal` added by @charliermarsh on 2024-01-27 04:17_

---

_Closed by @konstin on 2024-06-27 11:26_

---
