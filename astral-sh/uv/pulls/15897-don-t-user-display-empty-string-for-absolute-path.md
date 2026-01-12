```yaml
number: 15897
title: "Don't user display empty string for absolute path CWD"
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/path-display-fix
created_at: 2025-09-16T17:11:20Z
updated_at: 2025-09-16T17:49:09Z
url: https://github.com/astral-sh/uv/pull/15897
synced_at: 2026-01-12T16:12:00Z
```

# Don't user display empty string for absolute path CWD

---

_@konstin_

With the previous order an absolute path would become an empty string.

---

_Label `tracing` added by @konstin on 2025-09-16 17:13_

---

_Label `tracing` removed by @konstin on 2025-09-16 17:13_

---

_Label `error messages` added by @konstin on 2025-09-16 17:13_

---

_@zanieb approved on 2025-09-16 17:16_

---

_Comment by @konstin on 2025-09-16 17:23_

There's actually some existing non-tracing outputs affected by this, I've updated this snapshots.

---

_@zanieb reviewed on 2025-09-16 17:25_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:22904 on 2025-09-16 17:25_

I think we shouldn't change this, maybe we need to detect this case now? :(

---

_@zanieb reviewed on 2025-09-16 17:26_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:22904 on 2025-09-16 17:26_

I think it's just changing

```
#[error("{} has malformed dependency groups", if path.is_empty() && package.is_empty() {
    "Project".to_string()
} else if path.is_empty() {
    format!("Project `{package}`")
} else if package.is_empty() {
    format!("`{path}`")
} else {
    format!("Project `{package} @ {path}`")
})]
```

to handle `"."` instead of `""`

---

_Merged by @konstin on 2025-09-16 17:49_

---

_Closed by @konstin on 2025-09-16 17:49_

---

_Branch deleted on 2025-09-16 17:49_

---
