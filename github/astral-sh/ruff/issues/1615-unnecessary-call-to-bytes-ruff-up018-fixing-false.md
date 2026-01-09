---
number: 1615
title: "Unnecessary call to `bytes`Ruff(UP018) fixing false positive"
type: issue
state: closed
author: sbdchd
labels:
  - bug
assignees: []
created_at: 2023-01-04T02:11:06Z
updated_at: 2023-01-04T02:45:14Z
url: https://github.com/astral-sh/ruff/issues/1615
synced_at: 2026-01-07T13:12:14-06:00
---

# Unnecessary call to `bytes`Ruff(UP018) fixing false positive

---

_Issue opened by @sbdchd on 2023-01-04 02:11_

when hovering over the code the LSP warns:

```
Unnecessary call to `bytes` Ruff(UP018)
```

and auto fixing changes

```python
foo = str(b'{"foo":"bar"}')
```

to:

```python
foo = b'{"foo":"bar"}'
```

which has a different type




---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-04 02:24_

---

_Label `bug` added by @charliermarsh on 2023-01-04 02:29_

---

_Comment by @charliermarsh on 2023-01-04 02:29_

Thanks.

---

_Referenced in [astral-sh/ruff#1618](../../astral-sh/ruff/pulls/1618.md) on 2023-01-04 02:45_

---

_Closed by @charliermarsh on 2023-01-04 02:45_

---
