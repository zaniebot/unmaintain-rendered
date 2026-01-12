```yaml
number: 175
title: Avoid parsing unnecessary Metadata 2.1
type: issue
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
created_at: 2023-10-24T01:14:43Z
updated_at: 2023-11-22T13:25:37Z
url: https://github.com/astral-sh/uv/issues/175
synced_at: 2026-01-12T15:58:22Z
```

# Avoid parsing unnecessary Metadata 2.1

---

_@charliermarsh_

If you just remove all the fields that we don't need from the `Metadata21` struct, it cuts the cached resolution time by like ~30%. This is a free and trivial optimization we can do whenever we're ready.

---

_Label `performance` added by @charliermarsh on 2023-10-24 01:18_

---

_Added to milestone `Initial release` by @charliermarsh on 2023-10-24 19:19_

---

_Assigned to @konstin by @charliermarsh on 2023-11-09 14:26_

---

_Comment by @charliermarsh on 2023-11-09 14:26_

\cc @konstin 

---

_Comment by @charliermarsh on 2023-11-09 14:26_

The simplest thing here is literally to just remove the fields we don't need.

---

_Unassigned @konstin by @konstin on 2023-11-22 12:11_

---

_Assigned to @charliermarsh by @konstin on 2023-11-22 12:11_

---

_Closed by @charliermarsh on 2023-11-22 13:25_

---
