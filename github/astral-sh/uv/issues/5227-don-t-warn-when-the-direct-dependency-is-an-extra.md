---
number: 5227
title: "Don't warn when the direct dependency is an extra on root"
type: issue
state: closed
author: konstin
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-19T17:42:50Z
updated_at: 2024-07-28T18:35:20Z
url: https://github.com/astral-sh/uv/issues/5227
synced_at: 2026-01-07T13:12:17-06:00
---

# Don't warn when the direct dependency is an extra on root

---

_Issue opened by @konstin on 2024-07-19 17:42_

Got this error in packse, which is nonsensical since the root package doesn't need a bound:

```
$ uv lock --resolution lowest
warning: The direct dependency `packse[serve]` is unpinned. Consider setting a lower bound when using `--resolution-strategy lowest` to avoid using outdated versions.
```

---

_Label `bug` added by @konstin on 2024-07-19 17:42_

---

_Label `preview` added by @konstin on 2024-07-19 17:42_

---

_Assigned to @konstin by @konstin on 2024-07-19 17:42_

---

_Unassigned @konstin by @charliermarsh on 2024-07-28 17:51_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-28 17:51_

---

_Referenced in [astral-sh/uv#5518](../../astral-sh/uv/pulls/5518.md) on 2024-07-28 17:51_

---

_Closed by @charliermarsh on 2024-07-28 18:35_

---

_Closed by @charliermarsh on 2024-07-28 18:35_

---

_Referenced in [materialsproject/pymatgen#4073](../../materialsproject/pymatgen/pulls/4073.md) on 2024-10-13 04:09_

---

_Referenced in [astral-sh/uv#8155](../../astral-sh/uv/issues/8155.md) on 2024-10-13 04:20_

---
