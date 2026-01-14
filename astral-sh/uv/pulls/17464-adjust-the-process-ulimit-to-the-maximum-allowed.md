```yaml
number: 17464
title: Adjust the process ulimit to the maximum allowed on startup
type: pull_request
state: open
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: claude/add-ulimit-adjustment-pbq7y
created_at: 2026-01-14T14:49:11Z
updated_at: 2026-01-14T16:31:42Z
url: https://github.com/astral-sh/uv/pull/17464
synced_at: 2026-01-14T16:39:18Z
```

# Adjust the process ulimit to the maximum allowed on startup

---

_@zanieb_

Closes #16999

See the commentary at https://github.com/astral-sh/uv/issues/16999#issuecomment-3641417484 for precedence in other tools.

This is non-fatal on error, which may make it safe to roll out without preview gating.


---

_@zanieb reviewed on 2026-01-14 15:39_

---

_Review comment by @zanieb on `crates/uv-unix/src/resource_limits.rs`:27 on 2026-01-14 15:39_

```suggestion
/// 2. Code that iterates over all possible FDs, e.g., to close them, can timeout
```

---
