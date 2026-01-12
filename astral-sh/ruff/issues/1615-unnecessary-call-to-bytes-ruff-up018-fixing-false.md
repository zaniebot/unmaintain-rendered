```yaml
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
synced_at: 2026-01-12T15:54:41Z
```

# Unnecessary call to `bytes`Ruff(UP018) fixing false positive

---

_@sbdchd_

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

_Closed by @charliermarsh on 2023-01-04 02:45_

---
