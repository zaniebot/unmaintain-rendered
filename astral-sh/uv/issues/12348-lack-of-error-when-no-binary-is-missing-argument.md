```yaml
number: 12348
title: "Lack of error when `--no-binary` is missing argument in `requirements.txt`"
type: issue
state: closed
author: charliermarsh
labels:
  - good first issue
  - error messages
assignees: []
created_at: 2025-03-20T20:39:32Z
updated_at: 2025-03-20T23:25:29Z
url: https://github.com/astral-sh/uv/issues/12348
synced_at: 2026-01-12T16:01:01Z
```

# Lack of error when `--no-binary` is missing argument in `requirements.txt`

---

_@charliermarsh_

E.g., given this `requirements.txt`:

```
flask
--no-binary
```

Then `uv pip install -r requirements.txt` should error (as it does in `pip`).

---

_Label `good first issue` added by @charliermarsh on 2025-03-20 20:39_

---

_Label `error messages` added by @charliermarsh on 2025-03-20 20:39_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-20 22:37_

---

_Closed by @charliermarsh on 2025-03-20 23:25_

---

_Closed by @charliermarsh on 2025-03-20 23:25_

---
