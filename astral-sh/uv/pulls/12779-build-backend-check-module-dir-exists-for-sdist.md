```yaml
number: 12779
title: "Build backend: Check module dir exists for sdist build"
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: konsti/check-module-dir-exists
created_at: 2025-04-09T13:10:12Z
updated_at: 2025-04-09T15:47:02Z
url: https://github.com/astral-sh/uv/pull/12779
synced_at: 2026-01-12T16:10:23Z
```

# Build backend: Check module dir exists for sdist build

---

_@konstin_

Check that the source and module directory exist when build a source distribution, instead of delaying the check to building the wheel. This prevents building source distributions that can never be built into wheels.

---

_Label `bug` added by @konstin on 2025-04-09 13:10_

---

_Label `preview` added by @konstin on 2025-04-09 13:10_

---

_Review requested from @BurntSushi by @konstin on 2025-04-09 13:10_

---

_@Gankra approved on 2025-04-09 13:25_

Always nice to error more eagerly.

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/lib.rs`:555 on 2025-04-09 13:36_

It might be good to tweak the names here or beef up the docs just a tad. For example, the docs (and names in the implementation) suggest this both returns and accepts a module root.

---

_@BurntSushi approved on 2025-04-09 13:36_

Nice!

---

_Merged by @konstin on 2025-04-09 15:47_

---

_Closed by @konstin on 2025-04-09 15:47_

---

_Branch deleted on 2025-04-09 15:47_

---
