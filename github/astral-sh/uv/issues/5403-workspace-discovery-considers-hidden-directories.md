---
number: 5403
title: Workspace discovery considers hidden directories
type: issue
state: closed
author: mjclarke94
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-24T11:37:47Z
updated_at: 2024-07-24T16:21:35Z
url: https://github.com/astral-sh/uv/issues/5403
synced_at: 2026-01-07T13:12:17-06:00
---

# Workspace discovery considers hidden directories

---

_Issue opened by @mjclarke94 on 2024-07-24 11:37_

I have a simple folder structure for a workspace:
```
.
├── modules
│  ├── a
│  │  └── pyproject.toml
│  └── b
│     └── pyproject.toml
└── pyproject.toml
```

This works fine, but when I ran my unit test suite...

```
.
├── modules
│  ├── .hypothesis
│  ├── a
│  │  └── pyproject.toml
│  └── b
│     └── pyproject.toml
└── pyproject.toml
```

It broke. The various junk files spewed around by pytest, hypothesis, etc are left in hidden folders, which uv then expects to be a module if you have configured the workspace with a wildcard. It might be a more sensible default to ignore hidden directories.



---

_Label `bug` added by @konstin on 2024-07-24 12:02_

---

_Label `preview` added by @konstin on 2024-07-24 12:02_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-24 13:41_

---

_Referenced in [astral-sh/uv#5408](../../astral-sh/uv/pulls/5408.md) on 2024-07-24 14:24_

---

_Closed by @charliermarsh on 2024-07-24 16:21_

---
