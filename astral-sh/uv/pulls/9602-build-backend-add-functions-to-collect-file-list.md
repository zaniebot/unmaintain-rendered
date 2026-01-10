```yaml
number: 9602
title: "Build backend: Add functions to collect file list"
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/build-backend-list
created_at: 2024-12-03T10:46:32Z
updated_at: 2024-12-03T15:20:38Z
url: https://github.com/astral-sh/uv/pull/9602
synced_at: 2026-01-10T12:00:00Z
```

# Build backend: Add functions to collect file list

---

_Pull request opened by @konstin on 2024-12-03 10:46_

Using the directory writer trait, we can collect the files instead of writing them to a real sink. This builds up to a `uv build --list` similar to `cargo package --list`. It is not connected to the cli yet.

---

_Label `preview` added by @konstin on 2024-12-03 10:46_

---

_Review requested from @BurntSushi by @konstin on 2024-12-03 10:46_

---

_Merged by @konstin on 2024-12-03 10:58_

---

_Closed by @konstin on 2024-12-03 10:58_

---

_Branch deleted on 2024-12-03 10:58_

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/lib.rs`:78 on 2024-12-03 13:00_

It strikes me that this trait seems like a prime candidate for dynamic dispatch. But I think I see below that it is using monomorphization. The `close` method looks like that might be a bugger, but maybe there is a way to make it work. (Or give up taking `self` by value, regrettably.)

---

_@BurntSushi reviewed on 2024-12-03 13:00_

---

_Branch restored on 2024-12-03 15:20_

---
