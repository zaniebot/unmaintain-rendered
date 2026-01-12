```yaml
number: 4910
title: "`python_version !=` is incorrectly normalized"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-09T04:23:15Z
updated_at: 2024-07-09T16:04:28Z
url: https://github.com/astral-sh/uv/issues/4910
synced_at: 2026-01-12T15:58:52Z
```

# `python_version !=` is incorrectly normalized

---

_@charliermarsh_

On main, this passes:

```rust
assert_normalizes_to(
    "python_version != '3.8' and python_version < '3.10'",
    "python_version < '3.8' and python_version < '3.10' and python_version > '3.8'",
);
```

But the second term is not equivalent. It should be `python_version < '3.10' and (python_version < '3.8' or python_version > '3.8')`.


---

_Label `bug` added by @charliermarsh on 2024-07-09 04:23_

---

_Label `preview` added by @charliermarsh on 2024-07-09 04:23_

---

_Comment by @charliermarsh on 2024-07-09 04:23_

\cc @ibraheemdev 

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-09 04:43_

---

_Closed by @charliermarsh on 2024-07-09 16:04_

---

_Closed by @charliermarsh on 2024-07-09 16:04_

---
