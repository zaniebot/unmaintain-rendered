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
updated_at: 2026-01-15T05:41:06Z
url: https://github.com/astral-sh/uv/pull/17464
synced_at: 2026-01-15T05:50:48Z
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
