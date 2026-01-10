---
number: 173
title: Add a local dependency index
type: issue
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
created_at: 2023-10-23T21:52:44Z
updated_at: 2023-12-13T22:03:34Z
url: https://github.com/astral-sh/uv/issues/173
synced_at: 2026-01-10T01:23:03Z
---

# Add a local dependency index

---

_Issue opened by @charliermarsh on 2023-10-23 21:52_

To avoid repeatedly deserializing metadata, we could create a local index of (package, version)-to-requirements.

---

_Comment by @charliermarsh on 2023-10-23 21:52_

Needs benchmarking.

---

_Label `performance` added by @charliermarsh on 2023-10-24 01:18_

---

_Closed by @charliermarsh on 2023-12-13 22:03_

---
