```yaml
number: 5832
title: "`--config-file pyproject.toml` silently fails"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - configuration
assignees: []
created_at: 2024-08-06T21:15:50Z
updated_at: 2024-08-06T23:41:00Z
url: https://github.com/astral-sh/uv/issues/5832
synced_at: 2026-01-12T15:58:59Z
```

# `--config-file pyproject.toml` silently fails

---

_@charliermarsh_

Because we're very relaxed in how we parse the configuration, `pyproject.toml` adheres to the `uv.toml` schema (since all fields are optional). But this doesn't do what the user expects.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-06 21:15_

---

_Label `bug` added by @charliermarsh on 2024-08-06 21:15_

---

_Label `configuration` added by @charliermarsh on 2024-08-06 21:15_

---

_Closed by @charliermarsh on 2024-08-06 23:41_

---
