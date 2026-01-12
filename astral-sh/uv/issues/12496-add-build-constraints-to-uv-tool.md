```yaml
number: 12496
title: "Add `--build-constraints` to `uv tool`"
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2025-03-26T20:20:09Z
updated_at: 2025-04-14T13:26:59Z
url: https://github.com/astral-sh/uv/issues/12496
synced_at: 2026-01-12T16:01:05Z
```

# Add `--build-constraints` to `uv tool`

---

_@charliermarsh_

_No description provided._

---

_Label `enhancement` added by @charliermarsh on 2025-03-26 20:20_

---

_Label `help wanted` added by @charliermarsh on 2025-03-26 20:20_

---

_Comment by @x0rw on 2025-03-26 23:10_

what kind of constraints?
something like ``` uv tool --build-constraints "python>=3.8,<3.12 requests>=2.25,<3.0" ``` ?

---

_Comment by @charliermarsh on 2025-03-27 00:38_

This would be like the `--build-constraints` flag on `uv pip install`. So it would accept a `requirements.txt`-style file.

---

_Closed by @charliermarsh on 2025-04-14 13:26_

---

_Closed by @charliermarsh on 2025-04-14 13:26_

---
