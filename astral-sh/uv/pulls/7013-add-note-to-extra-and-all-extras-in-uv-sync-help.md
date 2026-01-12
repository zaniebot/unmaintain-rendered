```yaml
number: 7013
title: "Add note to `extra` and `all-extras` in `uv sync` help"
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/sync-extra
created_at: 2024-09-04T12:47:12Z
updated_at: 2024-09-04T15:05:22Z
url: https://github.com/astral-sh/uv/pull/7013
synced_at: 2026-01-12T16:07:38Z
```

# Add note to `extra` and `all-extras` in `uv sync` help

---

_@zanieb_

_No description provided._

---

_Label `documentation` added by @zanieb on 2024-09-04 12:47_

---

_@charliermarsh reviewed on 2024-09-04 13:24_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2373 on 2024-09-04 13:24_

```suggestion
    /// Note that optional dependencies are always included in the resolution; this option only
    /// affects the selection of packages to install.
```

---

_@charliermarsh reviewed on 2024-09-04 13:25_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2373 on 2024-09-04 13:25_

(If you don't take the suggestion, note that there's a typo or missing word somewhere in "Note that all optional dependencies always included in the resolution".)

---

_@charliermarsh approved on 2024-09-04 13:25_

---

_@zanieb reviewed on 2024-09-04 14:39_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2373 on 2024-09-04 14:39_

Thanks!

---

_Merged by @zanieb on 2024-09-04 15:05_

---

_Closed by @zanieb on 2024-09-04 15:05_

---

_Branch deleted on 2024-09-04 15:05_

---
