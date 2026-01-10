```yaml
number: 15545
title: "Read index credentials from the environment during `uv publish` checks"
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/fix-11836
created_at: 2025-08-27T08:44:14Z
updated_at: 2025-08-27T16:19:30Z
url: https://github.com/astral-sh/uv/pull/15545
synced_at: 2026-01-10T06:44:33Z
```

# Read index credentials from the environment during `uv publish` checks

---

_Pull request opened by @konstin on 2025-08-27 08:44_

We were previously missing the `index_locations.cache_index_credentials()` call in `uv publish` to load index credentials from the env.

See https://github.com/astral-sh/uv/issues/11836#issuecomment-3022735011
Fixes #11836



---

_Review requested from @zanieb by @konstin on 2025-08-27 08:44_

---

_Label `bug` added by @konstin on 2025-08-27 08:44_

---

_@konstin reviewed on 2025-08-27 08:44_

---

_Review comment by @konstin on `crates/uv/src/commands/publish.rs`:92 on 2025-08-27 08:44_

This line is the fix

---

_@zanieb approved on 2025-08-27 16:19_

---

_Merged by @zanieb on 2025-08-27 16:19_

---

_Closed by @zanieb on 2025-08-27 16:19_

---

_Branch deleted on 2025-08-27 16:19_

---

_Renamed from "Read index credentials from env for `uv publish`" to "Read index credentials from the environment during `uv publish` checks" by @zanieb on 2025-08-27 16:19_

---
