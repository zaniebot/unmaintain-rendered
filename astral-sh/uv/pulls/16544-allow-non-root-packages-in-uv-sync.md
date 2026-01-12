```yaml
number: 16544
title: "Allow non-root packages in `uv sync`"
type: pull_request
state: open
author: charliermarsh
labels:
  - enhancement
assignees: []
base: main
head: charlie/sync-non-member
created_at: 2025-11-01T02:16:18Z
updated_at: 2025-11-04T14:17:59Z
url: https://github.com/astral-sh/uv/pull/16544
synced_at: 2026-01-12T16:12:19Z
```

# Allow non-root packages in `uv sync`

---

_@charliermarsh_

## Summary

Closes #16248.


---

_Renamed from "Allow non-root packages in uv sync" to "Allow non-root packages in `uv sync`" by @charliermarsh on 2025-11-01 02:16_

---

_Review requested from @zanieb by @charliermarsh on 2025-11-01 02:16_

---

_Review requested from @konstin by @charliermarsh on 2025-11-01 02:16_

---

_Label `enhancement` added by @charliermarsh on 2025-11-01 02:16_

---

_Marked ready for review by @charliermarsh on 2025-11-01 19:39_

---

_Review comment by @konstin on `crates/uv/tests/it/sync.rs`:464 on 2025-11-04 13:14_

Is that remedied by selecting an extra? The error message looks thinner than what we usually have, we could e.g. show what anyio packages we got. (That's partially [my fault](https://github.com/astral-sh/uv/pull/16407) when this error message was still a sign for a faulty lockfile)

---

_@konstin approved on 2025-11-04 13:18_

---

_@charliermarsh reviewed on 2025-11-04 13:56_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/sync.rs`:464 on 2025-11-04 13:56_

I don't think so because we can't know what package the extra applies to. E.g., `uv sync --package anyio --extra foo` would, in theory, enable the `foo` extra on `anyio` (but that doesn't work, since we don't include the extras for all packages).

---
