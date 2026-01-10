---
number: 375
title: Cache only relevant wheel metadata
type: issue
state: closed
author: konstin
labels: []
assignees: []
created_at: 2023-11-09T14:03:50Z
updated_at: 2023-11-09T14:27:02Z
url: https://github.com/astral-sh/uv/issues/375
synced_at: 2026-01-10T01:23:04Z
---

# Cache only relevant wheel metadata

---

_Issue opened by @konstin on 2023-11-09 14:03_

Currently, we cache the entire `Metadata21`, which usually contains e.g. the project's readme. We should only store the relevant subset, [requirements](https://packaging.python.org/en/latest/specifications/core-metadata/#requires-dist-multiple-use) and [extras](https://packaging.python.org/en/latest/specifications/core-metadata/#provides-extra-multiple-use).

---

_Comment by @konstin on 2023-11-09 14:27_

Duplicate of #175

---

_Closed by @konstin on 2023-11-09 14:27_

---
