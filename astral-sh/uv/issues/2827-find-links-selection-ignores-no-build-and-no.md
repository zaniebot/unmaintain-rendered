---
number: 2827
title: "`--find-links` selection ignores `--no-build` and `--no-binary` "
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-04-05T01:48:43Z
updated_at: 2024-04-05T02:00:40Z
url: https://github.com/astral-sh/uv/issues/2827
synced_at: 2026-01-10T01:23:22Z
---

# `--find-links` selection ignores `--no-build` and `--no-binary` 

---

_Issue opened by @charliermarsh on 2024-04-05 01:48_

If you pass `--no-binary` with a `--find-links`, the resolver will still pick a wheel. Later, when we try to download it, we'll throw a hard error. Instead, the resolver should skip the wheel and choose a source distribution, if available.

---

_Label `bug` added by @charliermarsh on 2024-04-05 01:48_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-05 01:48_

---

_Referenced in [astral-sh/uv#2826](../../astral-sh/uv/pulls/2826.md) on 2024-04-05 01:48_

---

_Closed by @charliermarsh on 2024-04-05 02:00_

---
