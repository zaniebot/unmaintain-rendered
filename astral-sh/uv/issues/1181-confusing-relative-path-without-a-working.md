```yaml
number: 1181
title: "Confusing \"relative path without a working directory\" error"
type: issue
state: closed
author: zanieb
labels:
  - bug
  - error messages
assignees: []
created_at: 2024-01-30T02:58:56Z
updated_at: 2024-01-30T05:20:37Z
url: https://github.com/astral-sh/uv/issues/1181
synced_at: 2026-01-12T15:58:25Z
```

# Confusing "relative path without a working directory" error

---

_@zanieb_

e.g.

```
$ cargo run -- pip install packse@../../zanieb/packse
error: Failed to parse `packse@../../zanieb/packse`
  Caused by: relative path without a working directory: ../../zanieb/packse
```


---

_Label `error messages` added by @zanieb on 2024-01-30 02:58_

---

_Comment by @charliermarsh on 2024-01-30 03:03_

That's a bug (we shouldn't be calling `Requirement::from_str`).

---

_Label `bug` added by @zanieb on 2024-01-30 03:04_

---

_Closed by @charliermarsh on 2024-01-30 05:20_

---
