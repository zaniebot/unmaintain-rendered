---
number: 15142
title: "`uvx --from` support for extras and git at the same time"
type: issue
state: closed
author: bradbeattie
labels:
  - enhancement
assignees: []
created_at: 2025-08-07T16:44:38Z
updated_at: 2025-08-26T02:23:50Z
url: https://github.com/astral-sh/uv/issues/15142
synced_at: 2026-01-07T13:12:19-06:00
---

# `uvx --from` support for extras and git at the same time

---

_Issue opened by @bradbeattie on 2025-08-07 16:44_

### Summary

* https://docs.astral.sh/uv/guides/tools/#requesting-extras permits `uvx --from 'package[extra]'`.
* https://docs.astral.sh/uv/guides/tools/#requesting-different-sources permits `uvx --from git...`.
* But one cannot at the moment `uvx --from git...[extra]`.

### Example

`uvx --from git...[extra]` would install the dependency groups requested.

---

_Label `enhancement` added by @bradbeattie on 2025-08-07 16:44_

---

_Comment by @zanieb on 2025-08-07 18:13_

I believe this is supported, e.g., `uvx --from 'httpie[dev] @ git+https://github.com/httpie/cli' httpie`

I think we'd could add support for `--extra <name>` here like we've done in other places to make it easier to express.

---

_Closed by @charliermarsh on 2025-08-26 02:23_

---
