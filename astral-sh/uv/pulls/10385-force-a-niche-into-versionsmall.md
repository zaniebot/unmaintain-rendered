```yaml
number: 10385
title: "Force a niche into `VersionSmall`"
type: pull_request
state: merged
author: konstin
labels:
  - performance
assignees: []
merged: true
base: main
head: konsti/force-niche
created_at: 2025-01-08T09:48:30Z
updated_at: 2025-01-08T13:33:44Z
url: https://github.com/astral-sh/uv/pull/10385
synced_at: 2026-01-10T11:44:45Z
```

# Force a niche into `VersionSmall`

---

_Pull request opened by @konstin on 2025-01-08 09:48_

Force a niche into the aligned type so the [`Version`] enum is two words instead of three (x86_64). Rustc currently fails to optimize this on its own.

---

_Label `performance` added by @konstin on 2025-01-08 09:48_

---

_Review requested from @BurntSushi by @konstin on 2025-01-08 09:48_

---

_Review comment by @BurntSushi on `crates/uv-pep440/src/version.rs`:943 on 2025-01-08 13:25_

This is interesting. I get it. Even this only gives us one niche value (which I guess is all we need here), but I believe rustc itself is capable of expressing a _range_ of niche values: https://doc.rust-lang.org/src/core/num/nonzero.rs.html#50

Would be nice to have access to that more directly... Some day hopefully.

---

_@BurntSushi approved on 2025-01-08 13:25_

Sweeeeeeeeeet.

---

_@konstin reviewed on 2025-01-08 13:33_

---

_Review comment by @konstin on `crates/uv-pep440/src/version.rs`:943 on 2025-01-08 13:33_

We have a bunch of enum types (#10389 too) that could shrink a lot by rustc understanding niches across type nesting trees.

---

_Merged by @konstin on 2025-01-08 13:33_

---

_Closed by @konstin on 2025-01-08 13:33_

---

_Branch deleted on 2025-01-08 13:33_

---
