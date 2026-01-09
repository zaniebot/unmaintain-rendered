---
number: 12723
title: "The uv build backend should support `RUST_LOG` or something similar"
type: issue
state: closed
author: konstin
labels:
  - enhancement
assignees: []
created_at: 2025-04-07T17:30:31Z
updated_at: 2025-08-27T07:14:01Z
url: https://github.com/astral-sh/uv/issues/12723
synced_at: 2026-01-07T13:12:18-06:00
---

# The uv build backend should support `RUST_LOG` or something similar

---

_Issue opened by @konstin on 2025-04-07 17:30_

### Summary

The uv build backend writes log message through tracing, but there is currently no way to expose them when using PEP 517 (`uv build` can show them), hindering debugging. We should add a minimal tracing subscriber. Preferable we use a subscriber that supports fine grained selection such as the `RUST_LOG` matchers, but if that has too many dependencies a simple info/debug/trace selection also works.

### Example

_No response_

---

_Label `enhancement` added by @konstin on 2025-04-07 17:30_

---

_Assigned to @konstin by @konstin on 2025-04-07 17:30_

---

_Referenced in [astral-sh/uv#15533](../../astral-sh/uv/pulls/15533.md) on 2025-08-26 14:01_

---

_Closed by @konstin on 2025-08-27 07:14_

---
