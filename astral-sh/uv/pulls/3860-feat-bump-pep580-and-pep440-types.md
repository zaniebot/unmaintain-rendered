```yaml
number: 3860
title: "feat: bump pep580 and pep440 types"
type: pull_request
state: merged
author: tdejager
labels: []
assignees: []
merged: true
base: main
head: feat/bump-uv-types
created_at: 2024-05-27T07:17:18Z
updated_at: 2024-05-27T07:38:40Z
url: https://github.com/astral-sh/uv/pull/3860
synced_at: 2026-01-12T16:05:54Z
```

# feat: bump pep580 and pep440 types

---

_@tdejager_

This bumps the versions of pep580 and pep440 to coincide with the crates.io versions. While not strictly the same, the new types in uv us an `Inner` struct. Practically I've found I'm still able to use the patched versions, as can seen from the open PR here: https://github.com/prefix-dev/pixi/pull/1436.

Would be great if this bump can be done so we can keep combining the types :)

---

_@konstin approved on 2024-05-27 07:38_

---

_Merged by @konstin on 2024-05-27 07:38_

---

_Closed by @konstin on 2024-05-27 07:38_

---
