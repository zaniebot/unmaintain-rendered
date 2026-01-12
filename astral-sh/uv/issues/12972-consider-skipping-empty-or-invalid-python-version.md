```yaml
number: 12972
title: "Consider skipping empty or invalid `.python-version` files during discovery"
type: issue
state: open
author: zanieb
labels: []
assignees: []
created_at: 2025-04-18T16:30:04Z
updated_at: 2025-04-18T16:30:10Z
url: https://github.com/astral-sh/uv/issues/12972
synced_at: 2026-01-12T16:01:16Z
```

# Consider skipping empty or invalid `.python-version` files during discovery

---

_@zanieb_

We currently stop discovery at empty `.python-version` files, should we fallback to the global configuration or search for additional version files in parent directories?

See https://github.com/astral-sh/uv/pull/12909#discussion_r2050586857
            

---
