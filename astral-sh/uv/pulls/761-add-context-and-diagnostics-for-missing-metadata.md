```yaml
number: 761
title: "Add context and diagnostics for missing `METADATA`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/meta
created_at: 2024-01-03T22:37:06Z
updated_at: 2024-01-04T00:09:24Z
url: https://github.com/astral-sh/uv/pull/761
synced_at: 2026-01-12T16:04:10Z
```

# Add context and diagnostics for missing `METADATA`

---

_@charliermarsh_

Closes https://github.com/astral-sh/puffin/issues/717.

---

_Renamed from "Add context and diagnostics for missing METADATA" to "Add context and diagnostics for missing `METADATA`" by @charliermarsh on 2024-01-03 22:37_

---

_Review comment by @konstin on `crates/puffin-installer/src/site_packages.rs`:364 on 2024-01-03 22:39_

```suggestion
                "The package `{package}` is broken (unable to read `METADATA`): {}", path.display(),
```

Maybe also something like "please recreate the venv"?

---

_@konstin approved on 2024-01-03 22:39_

---

_Label `error messages` added by @charliermarsh on 2024-01-04 00:09_

---

_Merged by @charliermarsh on 2024-01-04 00:09_

---

_Closed by @charliermarsh on 2024-01-04 00:09_

---

_Branch deleted on 2024-01-04 00:09_

---
