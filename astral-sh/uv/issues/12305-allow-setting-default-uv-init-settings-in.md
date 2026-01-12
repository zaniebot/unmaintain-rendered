```yaml
number: 12305
title: "Allow setting default `uv init` settings in persistent configuration files"
type: issue
state: open
author: zanieb
labels:
  - enhancement
  - needs-design
assignees: []
created_at: 2025-03-19T01:00:54Z
updated_at: 2025-03-19T01:02:57Z
url: https://github.com/astral-sh/uv/issues/12305
synced_at: 2026-01-12T16:01:00Z
```

# Allow setting default `uv init` settings in persistent configuration files

---

_@zanieb_

### Summary

There have been a few requests to allow changing the `uv init` defaults in a persistent location, e.g., in a `uv.toml`. This issue tracks the general concept.

### Example

```toml
[init]
vcs = "none"
python = "3.14"
```

---

_Label `enhancement` added by @zanieb on 2025-03-19 01:00_

---

_Label `needs-design` added by @zanieb on 2025-03-19 01:00_

---
