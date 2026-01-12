```yaml
number: 16964
title: "Allow configuration of uv via loaded `.env` file"
type: issue
state: open
author: zanieb
labels:
  - needs-decision
assignees: []
created_at: 2025-12-03T13:40:19Z
updated_at: 2025-12-03T13:40:19Z
url: https://github.com/astral-sh/uv/issues/16964
synced_at: 2026-01-12T16:02:41Z
```

# Allow configuration of uv via loaded `.env` file

---

_@zanieb_

This is a tracking issue for a sentiment expressed in

- https://github.com/astral-sh/uv/issues/1384
- https://github.com/astral-sh/uv/issues/8862
- https://github.com/astral-sh/uv/issues/16926#issuecomment-3605670248 

When uv loads `.env` files, it does not respect `UV_*` variables within for its own configuration. There are some use-cases where it is desirable for uv to read configuration from variables in the file.

---

_Label `needs-decision` added by @zanieb on 2025-12-03 13:40_

---
