```yaml
number: 4383
title: "Use `impl AsRef<Path>` instead of type parameter"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/impl-as-ref
created_at: 2024-06-18T14:50:47Z
updated_at: 2024-06-18T15:54:08Z
url: https://github.com/astral-sh/uv/pull/4383
synced_at: 2026-01-12T16:06:12Z
```

# Use `impl AsRef<Path>` instead of type parameter

---

_@zanieb_

_No description provided._

---

_Label `internal` added by @zanieb on 2024-06-18 14:50_

---

_Review comment by @BurntSushi on `crates/uv/src/shell.rs`:70 on 2024-06-18 14:59_

I think you should just be using `impl AsRef<Path>` here. Both `&Path` and `PathBuf` implement `AsRef<Path>`.

And the same for below.

---

_@BurntSushi reviewed on 2024-06-18 14:59_

---

_@konstin approved on 2024-06-18 15:24_

---

_@zanieb reviewed on 2024-06-18 15:45_

---

_Review comment by @zanieb on `crates/uv/src/shell.rs`:70 on 2024-06-18 15:45_

Thought I needed the `&` for a known size but I guess not!

---

_Renamed from "Use `&impl AsRef<Path>` instead of type parameter" to "Use `impl AsRef<Path>` instead of type parameter" by @zanieb on 2024-06-18 15:46_

---

_Merged by @zanieb on 2024-06-18 15:54_

---

_Closed by @zanieb on 2024-06-18 15:54_

---

_Branch deleted on 2024-06-18 15:54_

---
