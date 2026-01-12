```yaml
number: 13465
title: "Build backend: Normalize glob paths"
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/normalize-build-backend-glob-paths
created_at: 2025-05-15T12:02:49Z
updated_at: 2025-05-15T15:19:05Z
url: https://github.com/astral-sh/uv/pull/13465
synced_at: 2026-01-12T16:10:41Z
```

# Build backend: Normalize glob paths

---

_@konstin_

Unlike OS APIs, glob inclusion checks don't work when there are relative path elements such as `./`. We normalize the path before using it for the glob.

Fixes #13407

---

_Review requested from @BurntSushi by @konstin on 2025-05-15 12:02_

---

_Label `bug` added by @konstin on 2025-05-15 12:02_

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/source_dist.rs`:123 on 2025-05-15 14:36_

I think this was my main concern here: whether path normalization could muck with the glob at all. Taking a look at `normalize_path` itself, I don't _think_ so. At least, I can't see a way for it to go bad.

---

_@BurntSushi approved on 2025-05-15 14:36_

---

_Merged by @konstin on 2025-05-15 15:19_

---

_Closed by @konstin on 2025-05-15 15:19_

---

_Branch deleted on 2025-05-15 15:19_

---
