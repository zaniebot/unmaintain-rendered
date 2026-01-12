```yaml
number: 12934
title: Consistent sorting in PythonInstallationKey by adjusting variant position
type: pull_request
state: closed
author: j178
labels: []
assignees: []
draft: true
base: main
head: list-order
created_at: 2025-04-17T06:41:56Z
updated_at: 2025-11-01T15:52:16Z
url: https://github.com/astral-sh/uv/pull/12934
synced_at: 2026-01-12T16:10:27Z
```

# Consistent sorting in PythonInstallationKey by adjusting variant position

---

_@j178_

## Summary

This PR shifts the variant to sit right after the version in `PythonInstallationKey`. For a key like `cpython-3.13.3+freethreaded-windows-x86_64-none`, the variant is essentially part of the version. This change helps make the sorting in the `uv python list` more logical and consistent.

~Depends on #12931~

---

_Review requested from @zanieb by @konstin on 2025-04-17 09:42_

---

_Converted to draft by @j178 on 2025-04-17 10:43_

---

_Assigned to @zanieb by @zanieb on 2025-04-17 13:36_

---

_Closed by @j178 on 2025-11-01 15:52_

---
