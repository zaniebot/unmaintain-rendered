```yaml
number: 16159
title: "Fix `uv python upgrade` replacement of installed binaries on pre-release to stable"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-up-pre
created_at: 2025-10-07T19:28:01Z
updated_at: 2025-10-07T20:29:44Z
url: https://github.com/astral-sh/uv/pull/16159
synced_at: 2026-01-10T06:36:15Z
```

# Fix `uv python upgrade` replacement of installed binaries on pre-release to stable

---

_Pull request opened by @zanieb on 2025-10-07 19:28_

_No description provided._

---

_Label `bug` added by @zanieb on 2025-10-07 19:28_

---

_Marked ready for review by @zanieb on 2025-10-07 20:02_

---

_Review requested from @geofft by @zanieb on 2025-10-07 20:07_

---

_Review comment by @geofft on `crates/uv-python/src/managed.rs`:697 on 2025-10-07 20:08_

Trying to decide if I like this better:
```suggestion
            return match (self.key.prerelease, other.key.prerelease) {
                // Require a newer pre-release, if present on both
                (Some(self_pre), Some(other_pre)) => self.pre > other.pre,
                // Allow upgrade from pre-release to stable
                (None, Some(_)) => true,
                // Do not upgrade from pre-release to stable, or for matching versions
                (_, None) => false,
            };
```

---

_@geofft approved on 2025-10-07 20:09_

---

_@zanieb reviewed on 2025-10-07 20:17_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:697 on 2025-10-07 20:17_

I don't mind that

---

_Merged by @zanieb on 2025-10-07 20:29_

---

_Closed by @zanieb on 2025-10-07 20:29_

---

_Branch deleted on 2025-10-07 20:29_

---
