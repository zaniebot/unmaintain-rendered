---
number: 17199
title: "Add a `query` command to uv cache for inspecting local cache, similar to `cargo-cache`"
type: issue
state: open
author: zhangzhenxiang666
labels:
  - enhancement
  - wish
assignees: []
created_at: 2025-12-20T12:33:32Z
updated_at: 2026-01-06T19:24:31Z
url: https://github.com/astral-sh/uv/issues/17199
synced_at: 2026-01-07T13:12:19-06:00
---

# Add a `query` command to uv cache for inspecting local cache, similar to `cargo-cache`

---

_Issue opened by @zhangzhenxiang666 on 2025-12-20 12:33_

### Summary

I manage many Python projects, some of which share the same third-party dependencies. When installing dependencies, if I want to reuse the exact same versions already used in a previous project, I currently have to manually open that project’s pyproject.toml file and copy the dependency versions. Otherwise, uv will default to fetching the latest available versions (unless the dependency is already locked to the latest version).

It would be very helpful if uv could provide a query command—similar to cargo-cache—to inspect what versions of packages are already cached locally. This would make it much easier to reuse consistent dependency versions across projects without manual lookups.

### Example

_No response_

---

_Label `enhancement` added by @zhangzhenxiang666 on 2025-12-20 12:33_

---

_Label `wish` added by @konstin on 2026-01-06 19:24_

---
