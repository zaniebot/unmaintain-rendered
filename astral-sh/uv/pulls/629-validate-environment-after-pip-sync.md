```yaml
number: 629
title: "Validate environment after `pip-sync`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/validate-venv
created_at: 2023-12-13T03:35:43Z
updated_at: 2023-12-13T08:13:45Z
url: https://github.com/astral-sh/uv/pull/629
synced_at: 2026-01-12T16:04:05Z
```

# Validate environment after `pip-sync`

---

_@charliermarsh_

Not 100% sure that we actually want to do this, it seems reasonable though.

Closes https://github.com/astral-sh/puffin/issues/410.


---

_Review requested from @zanieb by @charliermarsh on 2023-12-13 03:35_

---

_Review requested from @konstin by @charliermarsh on 2023-12-13 03:35_

---

_@charliermarsh reviewed on 2023-12-13 03:47_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/pip_sync.rs`:772 on 2023-12-13 03:47_

(This has platform-specific dependencies that made the warnings inconsistent across macOS and CI.)

---

_@konstin approved on 2023-12-13 08:13_

---

_Merged by @konstin on 2023-12-13 08:13_

---

_Closed by @konstin on 2023-12-13 08:13_

---

_Branch deleted on 2023-12-13 08:13_

---
