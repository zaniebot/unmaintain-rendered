```yaml
number: 13566
title: Fix version json tests to work outside git checkout
type: pull_request
state: merged
author: mgorny
labels:
  - internal
assignees: []
merged: true
base: main
head: commit-info-nogit
created_at: 2025-05-21T07:43:31Z
updated_at: 2025-05-21T19:59:53Z
url: https://github.com/astral-sh/uv/pull/13566
synced_at: 2026-01-10T11:10:41Z
```

# Fix version json tests to work outside git checkout

---

_Pull request opened by @mgorny on 2025-05-21 07:43_

## Summary

Fix the two version json tests to account for the possibility that uv was built outside a git checkout (e.g. from an unpacked git archive) and therefore does not have the commit info available.  This approach uses separate snapshots for the two cases, as suggested in discussion of pull request #13251.

Fixes #13212

## Test Plan

1. `cargo test` in a git clone.
2. `cargo clean`, moved `.git` away, `cargo test` again.

---

_Review requested from @Gankra by @konstin on 2025-05-21 09:05_

---

_Assigned to @Gankra by @konstin on 2025-05-21 09:05_

---

_Review comment by @Gankra on `crates/uv/build.rs`:23 on 2025-05-21 17:13_

Doing this in a build.rs seems like an extremely heavy hammer that might mess up build caching -- can we just do this .git check in the tests themselves?

---

_@Gankra approved on 2025-05-21 17:15_

Oh geez, apologies, finishing this work slipped through the cracks!

---

_@mgorny reviewed on 2025-05-21 19:07_

---

_Review comment by @mgorny on `crates/uv/build.rs`:23 on 2025-05-21 19:07_

I suppose so, but I have no clue how to get the right path :-). I should probably mention I don't really know Cargo/Rust, and I'm just bashing things randomly until they work.

---

_Review comment by @Gankra on `crates/uv/build.rs`:23 on 2025-05-21 19:25_

Ah ok no prob, I'll take a quick crack at it :)

---

_@Gankra reviewed on 2025-05-21 19:25_

---

_@mgorny reviewed on 2025-05-21 19:46_

---

_Review comment by @mgorny on `crates/uv/build.rs`:23 on 2025-05-21 19:46_

Thanks!

---

_Merged by @Gankra on 2025-05-21 19:59_

---

_Closed by @Gankra on 2025-05-21 19:59_

---

_Label `internal` added by @Gankra on 2025-05-21 19:59_

---
