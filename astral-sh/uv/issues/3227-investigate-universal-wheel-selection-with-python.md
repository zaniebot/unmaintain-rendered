```yaml
number: 3227
title: "Investigate universal wheel selection with `--python-platform`"
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2024-04-23T22:21:20Z
updated_at: 2024-04-23T23:28:15Z
url: https://github.com/astral-sh/uv/issues/3227
synced_at: 2026-01-12T15:58:42Z
```

# Investigate universal wheel selection with `--python-platform`

---

_@charliermarsh_

> uv pip compile requirements.in --python-platform=macos also fails for open3d because the ARM64 wheels are only available for MacOS 13.0+ (or universal for 10.15+) while uv specifies MacOS 11.0 for ARM64.

See: https://github.com/astral-sh/uv/pull/3111#discussion_r1576860349

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-23 22:21_

---

_Renamed from "Investigate universal wheel seleciton with `--python-platform`" to "Investigate universal wheel selection with `--python-platform`" by @charliermarsh on 2024-04-23 22:21_

---

_Closed by @charliermarsh on 2024-04-23 23:28_

---
