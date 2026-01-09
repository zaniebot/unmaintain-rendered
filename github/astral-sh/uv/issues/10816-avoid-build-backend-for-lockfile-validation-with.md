---
number: 10816
title: Avoid build backend for lockfile validation with Hatchling with recursive extras and dynamic metadata
type: issue
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
created_at: 2025-01-21T14:19:04Z
updated_at: 2025-01-21T21:01:22Z
url: https://github.com/astral-sh/uv/issues/10816
synced_at: 2026-01-07T13:12:18-06:00
---

# Avoid build backend for lockfile validation with Hatchling with recursive extras and dynamic metadata

---

_Issue opened by @charliermarsh on 2025-01-21 14:19_

This is a mouthful but the PR will make it clearer. Partly a note to self: we can avoid the "slow path" from #10797 if we compare both the flattened and unflattened metadata.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-21 14:19_

---

_Label `performance` added by @charliermarsh on 2025-01-21 14:19_

---

_Referenced in [astral-sh/uv#10823](../../astral-sh/uv/pulls/10823.md) on 2025-01-21 20:33_

---

_Closed by @charliermarsh on 2025-01-21 21:01_

---
