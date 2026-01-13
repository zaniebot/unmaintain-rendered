```yaml
number: 17440
title: "Add [tool.uv] metadata to pylock.toml export"
type: pull_request
state: open
author: gaborbernat
labels: []
assignees: []
draft: true
base: main
head: pylock-tool-uv
created_at: 2026-01-13T16:20:10Z
updated_at: 2026-01-13T21:17:58Z
url: https://github.com/astral-sh/uv/pull/17440
synced_at: 2026-01-13T21:36:42Z
```

# Add [tool.uv] metadata to pylock.toml export

---

_@gaborbernat_

Add tool.uv section to PEP 751 pylock.toml output containing:
- version: Current uv version for reproducibility tracking
- command: Command line args used to generate the lock file


---

_Review comment by @zanieb on `crates/uv-resolver/src/lock/export/pylock_toml.rs`:978 on 2026-01-13 21:17_

This should use `uv-version::version`

---

_@zanieb reviewed on 2026-01-13 21:17_

---

_@zanieb reviewed on 2026-01-13 21:17_

---

_Review comment by @zanieb on `crates/uv-resolver/src/lock/export/pylock_toml.rs`:985 on 2026-01-13 21:17_

What's happening here? Stripping the executable path?

---
