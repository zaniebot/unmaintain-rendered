```yaml
number: 6540
title: "Add `uv sync --no-install-package` to skip installation of specific packages"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/no-install-package
created_at: 2024-08-23T19:25:20Z
updated_at: 2024-08-23T20:48:05Z
url: https://github.com/astral-sh/uv/pull/6540
synced_at: 2026-01-10T13:09:51Z
```

# Add `uv sync --no-install-package` to skip installation of specific packages

---

_Pull request opened by @zanieb on 2024-08-23 19:25_

Extends #6538 / #6539
See #4028

Allows excluding arbitrary packages from the sync.

---

_@charliermarsh reviewed on 2024-08-23 19:47_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:416 on 2024-08-23 19:47_

It could be nice to capture these all together in a single struct, like BuildOptions. But not blocking.

---

_@charliermarsh approved on 2024-08-23 19:47_

---

_@zanieb reviewed on 2024-08-23 20:00_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:416 on 2024-08-23 20:00_

Yes totally, on my mind too when I turned off a clippy warning.

---

_@zanieb reviewed on 2024-08-23 20:42_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:416 on 2024-08-23 20:42_

https://github.com/astral-sh/uv/issues/6545

---

_Merged by @zanieb on 2024-08-23 20:48_

---

_Closed by @zanieb on 2024-08-23 20:48_

---

_Branch deleted on 2024-08-23 20:48_

---
