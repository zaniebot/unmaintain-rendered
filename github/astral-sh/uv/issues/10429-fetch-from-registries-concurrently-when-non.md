---
number: 10429
title: "Fetch from registries concurrently when non-default `--index-strategy` is present"
type: issue
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
created_at: 2025-01-09T14:21:30Z
updated_at: 2025-01-09T17:45:22Z
url: https://github.com/astral-sh/uv/issues/10429
synced_at: 2026-01-07T13:12:18-06:00
---

# Fetch from registries concurrently when non-default `--index-strategy` is present

---

_Issue opened by @charliermarsh on 2025-01-09 14:21_

Within the `RegistryClient`, for a single package, we can fetch from all registries concurrently if the user has an unsafe `--index-strategy`, since we _know_ we'll fetch from all of them.

---

_Label `performance` added by @charliermarsh on 2025-01-09 14:21_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-09 14:31_

---

_Referenced in [astral-sh/uv#10432](../../astral-sh/uv/pulls/10432.md) on 2025-01-09 14:59_

---

_Closed by @charliermarsh on 2025-01-09 17:45_

---
