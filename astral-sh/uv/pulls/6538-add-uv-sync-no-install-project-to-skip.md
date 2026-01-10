```yaml
number: 6538
title: "Add `uv sync --no-install-project` to skip installation of the project"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/no-install-root
created_at: 2024-08-23T18:46:49Z
updated_at: 2024-08-24T15:44:49Z
url: https://github.com/astral-sh/uv/pull/6538
synced_at: 2026-01-10T13:09:51Z
```

# Add `uv sync --no-install-project` to skip installation of the project

---

_Pull request opened by @zanieb on 2024-08-23 18:46_

See #4028

A smaller version of https://github.com/astral-sh/uv/pull/6398

---

_Label `enhancement` added by @zanieb on 2024-08-23 18:46_

---

_Comment by @zanieb on 2024-08-23 18:49_

Arguably could just be `--no-install-project`? That'd match our terminology elsewhere. "root" doesn't really exist in our documentation.

---

_Comment by @charliermarsh on 2024-08-23 19:44_

Using “project” seems ok yeah

---

_@charliermarsh approved on 2024-08-23 19:45_

---

_Renamed from "Add `uv sync --no-install-root` to skip installation of the root project" to "Add `uv sync --no-install-project` to skip installation of the project" by @zanieb on 2024-08-23 20:08_

---

_Merged by @zanieb on 2024-08-23 20:19_

---

_Closed by @zanieb on 2024-08-23 20:19_

---

_Branch deleted on 2024-08-23 20:19_

---

_@gdamjan reviewed on 2024-08-24 15:44_

---

_Review comment by @gdamjan on `docs/guides/integration/docker.md`:63 on 2024-08-24 15:44_

should this be `--locked` instead, so as for the, supposedly production, Docker image fail to build if `pyproject.toml` and `uv.lock` are not in sync?

below too?

---
