```yaml
number: 13536
title: "Add support for `uvx pypy`"
type: issue
state: closed
author: zanieb
labels:
  - enhancement
assignees: []
created_at: 2025-05-19T13:51:02Z
updated_at: 2025-05-30T16:12:40Z
url: https://github.com/astral-sh/uv/issues/13536
synced_at: 2026-01-10T03:41:47Z
```

# Add support for `uvx pypy`

---

_Issue opened by @zanieb on 2025-05-19 13:51_

### Summary

This currently only supports `python` and `pythonw`

https://github.com/astral-sh/uv/blob/149102a4e7689e1e5cd7eca51fffcfbda4e758d8/crates/uv/src/commands/tool/mod.rs#L51

We do have a later invocation of `PythonRequest::parse`, which will handle these generic cases

https://github.com/astral-sh/uv/blob/c5032aee80d5a666a45ec5e095b8c8abeffc11fb/crates/uv/src/commands/tool/run.rs#L725

but it's not used here

### Example

We should support all implementations here

```
uvx pypy
uvx pypy@3.8
uvx graalpy
uvx cpython
```

---

_Label `enhancement` added by @zanieb on 2025-05-19 13:51_

---

_Comment by @CodeMan62 on 2025-05-20 18:33_

I will try ths for sure and make a PR asap 

---

_Comment by @zanieb on 2025-05-20 18:54_

Hey @CodeMan62, I think @oconnor663 is already starting on this one.

---

_Assigned to @oconnor663 by @konstin on 2025-05-20 18:54_

---

_Comment by @CodeMan62 on 2025-05-20 18:58_

No worries


---

_Comment by @oconnor663 on 2025-05-20 19:13_

Yep on it! I should've grabbed the "assignee".

---

_Closed by @oconnor663 on 2025-05-30 16:12_

---
