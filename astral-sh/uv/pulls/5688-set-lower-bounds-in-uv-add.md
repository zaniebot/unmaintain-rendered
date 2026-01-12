```yaml
number: 5688
title: "Set lower bounds in `uv add`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: charlie/add-lower-bound
created_at: 2024-08-01T14:58:36Z
updated_at: 2024-08-01T17:06:58Z
url: https://github.com/astral-sh/uv/pull/5688
synced_at: 2026-01-12T16:06:57Z
```

# Set lower bounds in `uv add`

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/5178.


---

_Label `enhancement` added by @charliermarsh on 2024-08-01 14:58_

---

_Label `preview` added by @charliermarsh on 2024-08-01 14:58_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/add.rs`:302 on 2024-08-01 14:59_

Worth reviewing the criteria here for _when_ we set a lower bound: (1) never for existing dependencies, it must be a new dependency in `uv add`; (2) never when the user provides a URL.

---

_@charliermarsh reviewed on 2024-08-01 14:59_

---

_Review requested from @zanieb by @charliermarsh on 2024-08-01 15:15_

---

_Review requested from @konstin by @charliermarsh on 2024-08-01 15:15_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-08-01 15:15_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/project/add.rs`:158 on 2024-08-01 15:34_

I think this is outdated?

---

_@ibraheemdev approved on 2024-08-01 15:34_

---

_Merged by @charliermarsh on 2024-08-01 17:06_

---

_Closed by @charliermarsh on 2024-08-01 17:06_

---

_Branch deleted on 2024-08-01 17:06_

---
