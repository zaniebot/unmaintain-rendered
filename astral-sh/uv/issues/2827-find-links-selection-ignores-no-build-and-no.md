```yaml
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
synced_at: 2026-01-12T15:58:40Z
```

# `--find-links` selection ignores `--no-build` and `--no-binary` 

---

_@charliermarsh_

If you pass `--no-binary` with a `--find-links`, the resolver will still pick a wheel. Later, when we try to download it, we'll throw a hard error. Instead, the resolver should skip the wheel and choose a source distribution, if available.

---

_Label `bug` added by @charliermarsh on 2024-04-05 01:48_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-05 01:48_

---

_Closed by @charliermarsh on 2024-04-05 02:00_

---
