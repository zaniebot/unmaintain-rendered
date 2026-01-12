```yaml
number: 14755
title: "Support `extras` and `dependency_groups` markers on `uv pip install` and `uv pip sync`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - compatibility
assignees: []
merged: true
base: main
head: charlie/dep-groups-install
created_at: 2025-07-20T17:42:41Z
updated_at: 2025-07-21T12:48:48Z
url: https://github.com/astral-sh/uv/pull/14755
synced_at: 2026-01-12T16:11:23Z
```

# Support `extras` and `dependency_groups` markers on `uv pip install` and `uv pip sync`

---

_@charliermarsh_

## Summary

We don't yet support writing these, but we can at least read them (which, e.g., allows you to install PDM-exported `pylock.toml` files with uv, since PDM _always_ writes a default group).

Closes #14740.


---

_Marked ready for review by @charliermarsh on 2025-07-20 18:17_

---

_Review requested from @zanieb by @charliermarsh on 2025-07-20 18:19_

---

_Review requested from @konstin by @charliermarsh on 2025-07-20 18:19_

---

_Label `enhancement` added by @charliermarsh on 2025-07-20 18:19_

---

_Label `compatibility` added by @charliermarsh on 2025-07-20 18:19_

---

_Review comment by @konstin on `crates/uv/tests/it/pip_install.rs`:11403 on 2025-07-21 08:47_

We shouldn't use this with a Python 3.12 test context

---

_@konstin approved on 2025-07-21 08:58_

---

_Merged by @charliermarsh on 2025-07-21 12:48_

---

_Closed by @charliermarsh on 2025-07-21 12:48_

---

_Branch deleted on 2025-07-21 12:48_

---
