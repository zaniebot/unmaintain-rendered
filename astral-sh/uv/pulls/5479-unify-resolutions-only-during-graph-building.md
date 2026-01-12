```yaml
number: 5479
title: Unify resolutions only during graph building
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/unify-resolutions-later
created_at: 2024-07-26T13:05:49Z
updated_at: 2024-08-23T15:35:24Z
url: https://github.com/astral-sh/uv/pull/5479
synced_at: 2026-01-12T16:06:50Z
```

# Unify resolutions only during graph building

---

_@konstin_

With our previous eager union, we were losing the fork markers. We now carry this information into the resolution graph construction and, in the next step, can read the markers there.

Part of https://github.com/astral-sh/uv/issues/5180#issuecomment-2247696198

---

_Review requested from @BurntSushi by @konstin on 2024-07-26 13:05_

---

_Label `preview` added by @konstin on 2024-07-26 13:18_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:2428 on 2024-07-26 14:27_

Ah nice, because each resolution is only allowed one version of a package, and `Resolution` no longer needs to represent the union of all resolutions since we merge them right into the graph?

---

_@BurntSushi approved on 2024-07-26 14:29_

This makes sense to me!

---

_@konstin reviewed on 2024-07-26 14:29_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:2428 on 2024-07-26 14:29_

yep

---

_Merged by @konstin on 2024-07-26 14:29_

---

_Closed by @konstin on 2024-07-26 14:29_

---

_Branch deleted on 2024-07-26 14:29_

---
