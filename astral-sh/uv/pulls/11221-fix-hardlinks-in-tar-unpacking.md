```yaml
number: 11221
title: Fix hardlinks in tar unpacking
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/hardlinks-tar-unpack
created_at: 2025-02-04T17:10:06Z
updated_at: 2025-02-04T17:38:24Z
url: https://github.com/astral-sh/uv/pull/11221
synced_at: 2026-01-10T11:10:34Z
```

# Fix hardlinks in tar unpacking

---

_Pull request opened by @konstin on 2025-02-04 17:10_

In https://github.com/astral-sh/tokio-tar/pull/2, we accidentally changed the `target_base` from the target base to the parent of the file. This would cause hardlink unpacking to fail.

Example: A hardlink at `hardlinked-0.1.0/pyproject.toml` pointing to `hardlinked-0.1.0/pyproject.toml.real` would try pointing to `hardlinked-0.1.0/hardlinked-0.1.0/pyproject.toml.real` instead and fail the unpacking.

The actual fix is astral-tokio-tar, on the uv side there are only tests.

Fixes #11213

---

_Label `bug` added by @konstin on 2025-02-04 17:10_

---

_Review requested from @charliermarsh by @konstin on 2025-02-04 17:10_

---

_@charliermarsh approved on 2025-02-04 17:11_

Thank you!

---

_Merged by @konstin on 2025-02-04 17:38_

---

_Closed by @konstin on 2025-02-04 17:38_

---

_Branch deleted on 2025-02-04 17:38_

---
