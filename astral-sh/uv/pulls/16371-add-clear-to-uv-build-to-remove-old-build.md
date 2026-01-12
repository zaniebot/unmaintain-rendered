```yaml
number: 16371
title: "Add `--clear` to `uv build` to remove old build artifacts"
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/uv-build-clear
created_at: 2025-10-20T10:04:05Z
updated_at: 2025-10-27T18:15:18Z
url: https://github.com/astral-sh/uv/pull/16371
synced_at: 2026-01-12T16:12:14Z
```

# Add `--clear` to `uv build` to remove old build artifacts

---

_@konstin_

Add `uv build --clear` that behaves like `rm -r ./dist && uv build` to clear artifacts from previous builds. This avoids accidentally publishing the wrong artifacts and removes accumulated old builds.

See https://github.com/astral-sh/uv/issues/10293#issuecomment-3405904013
Fixes #10293


---

_Review requested from @zanieb by @konstin on 2025-10-20 10:04_

---

_Label `enhancement` added by @konstin on 2025-10-20 10:04_

---

_@zanieb approved on 2025-10-23 20:12_

---

_@zanieb reviewed on 2025-10-23 20:13_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2638 on 2025-10-23 20:13_

I think this needs to be clearer that it'll remove _stale_ build artifacts and not the new ones :)

---

_Merged by @konstin on 2025-10-27 18:15_

---

_Closed by @konstin on 2025-10-27 18:15_

---

_Branch deleted on 2025-10-27 18:15_

---
