```yaml
number: 599
title: "Use `prepare_metadata_for_build_wheel` in resolver"
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
  - performance
assignees: []
created_at: 2023-12-09T04:26:54Z
updated_at: 2024-01-10T00:07:39Z
url: https://github.com/astral-sh/uv/issues/599
synced_at: 2026-01-12T15:58:24Z
```

# Use `prepare_metadata_for_build_wheel` in resolver

---

_@charliermarsh_

_No description provided._

---

_Label `enhancement` added by @charliermarsh on 2023-12-09 04:26_

---

_Label `performance` added by @charliermarsh on 2023-12-09 04:26_

---

_Added to milestone `Initial release` by @charliermarsh on 2023-12-09 04:27_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-09 16:19_

---

_Comment by @konstin on 2023-12-11 13:07_

Note that this call changes the state, you must pass the output as `metadata_directory` to `build_wheel` (https://peps.python.org/pep-0517/#build-wheel

---

_Unassigned @charliermarsh by @charliermarsh on 2023-12-12 17:41_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-08 04:30_

---

_Closed by @charliermarsh on 2024-01-10 00:07_

---
