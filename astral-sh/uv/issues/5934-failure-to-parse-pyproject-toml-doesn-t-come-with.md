```yaml
number: 5934
title: "Failure to parse `pyproject.toml` doesn't come with error even in verbose mode"
type: issue
state: closed
author: zanieb
labels:
  - error messages
  - tracing
assignees: []
created_at: 2024-08-08T21:21:47Z
updated_at: 2024-08-11T21:13:15Z
url: https://github.com/astral-sh/uv/issues/5934
synced_at: 2026-01-10T04:53:49Z
```

# Failure to parse `pyproject.toml` doesn't come with error even in verbose mode

---

_Issue opened by @zanieb on 2024-08-08 21:21_

e.g.

```
[tool.uv]
foobar = false
```

```
❯ uv venv --preview -v
warning: Failed to parse `pyproject.toml` during settings discovery; skipping...
DEBUG uv 0.2.34
....
```

---

_Comment by @zanieb on 2024-08-08 21:21_

I'd love to know what was invalid!

---

_Label `error messages` added by @zanieb on 2024-08-08 21:22_

---

_Label `tracing` added by @zanieb on 2024-08-08 21:22_

---

_Closed by @charliermarsh on 2024-08-11 21:13_

---

_Closed by @charliermarsh on 2024-08-11 21:13_

---
