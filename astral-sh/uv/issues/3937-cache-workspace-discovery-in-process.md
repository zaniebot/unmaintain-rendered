---
number: 3937
title: Cache workspace discovery in-process
type: issue
state: open
author: konstin
labels:
  - performance
assignees: []
created_at: 2024-05-31T12:12:50Z
updated_at: 2024-09-14T22:23:48Z
url: https://github.com/astral-sh/uv/issues/3937
synced_at: 2026-01-10T01:23:32Z
---

# Cache workspace discovery in-process

---

_Issue opened by @konstin on 2024-05-31 12:12_

Currently, every time we compute a `Metadata` instance, we performance workspace discovery anew. Instead, we should cache the paths we know are part of a workspace and reuse them. 

---

_Label `preview` added by @konstin on 2024-05-31 12:12_

---

_Label `preview` removed by @zanieb on 2024-08-20 18:23_

---

_Label `performance` added by @charliermarsh on 2024-09-14 22:23_

---
