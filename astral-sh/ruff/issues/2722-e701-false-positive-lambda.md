```yaml
number: 2722
title: "E701 false positive: lambda"
type: issue
state: closed
author: spaceone
labels:
  - bug
assignees: []
created_at: 2023-02-10T17:44:42Z
updated_at: 2023-02-10T20:21:08Z
url: https://github.com/astral-sh/ruff/issues/2722
synced_at: 2026-01-12T15:54:43Z
```

# E701 false positive: lambda

---

_@spaceone_

`lambda foo: foo` is detected as E701

---

_Comment by @charliermarsh on 2023-02-10 17:52_

This hasn't shipped right? You're running off `main`?

---

_Label `bug` added by @charliermarsh on 2023-02-10 18:10_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-10 18:10_

---

_Comment by @charliermarsh on 2023-02-10 18:12_

I really wanted to avoid basing these rules off `logical_lines` but we might have to reimplement them that way. They'd then have to be disabled by default.

---

_Closed by @charliermarsh on 2023-02-10 20:21_

---
