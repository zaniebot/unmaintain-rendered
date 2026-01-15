```yaml
number: 17464
title: Adjust the process ulimit to the maximum allowed on startup
type: pull_request
state: open
author: zanieb
labels:
  - preview
assignees: []
base: main
head: claude/add-ulimit-adjustment-pbq7y
created_at: 2026-01-14T14:49:11Z
updated_at: 2026-01-15T16:43:30Z
url: https://github.com/astral-sh/uv/pull/17464
synced_at: 2026-01-15T16:50:35Z
```

# Adjust the process ulimit to the maximum allowed on startup

---

_@zanieb_

Closes #16999

See the commentary at https://github.com/astral-sh/uv/issues/16999#issuecomment-3641417484 for precedence in other tools.

This is non-fatal on error, but there are various scary behaviors from increased ulimit sizes so I've gated this with preview.

---

_@zanieb reviewed on 2026-01-14 15:39_

---

_Review comment by @zanieb on `crates/uv-unix/src/resource_limits.rs`:27 on 2026-01-14 15:39_

```suggestion
/// 2. Code that iterates over all possible FDs, e.g., to close them, can timeout
```

---

_Label `preview` added by @zanieb on 2026-01-14 21:25_

---

_Marked ready for review by @zanieb on 2026-01-15 05:41_

---

_Review comment by @konstin on `crates/uv-unix/src/resource_limits.rs`:32 on 2026-01-15 16:27_

:100: for the extensive comment

---

_Review comment by @konstin on `crates/uv-unix/src/resource_limits.rs`:41 on 2026-01-15 16:28_

This should return an Option (or nothing, we're not using the value)

---

_@zsol approved on 2026-01-15 16:30_

lgtm

---

_@konstin approved on 2026-01-15 16:31_

---

_@zanieb reviewed on 2026-01-15 16:43_

---

_Review comment by @zanieb on `crates/uv-unix/src/resource_limits.rs`:41 on 2026-01-15 16:43_

I imagine we might use it to tune concurrency limits in the future? I think that's the general idea in the other packages.

---
