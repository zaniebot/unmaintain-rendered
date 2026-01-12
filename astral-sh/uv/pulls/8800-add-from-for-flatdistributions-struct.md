```yaml
number: 8800
title: "Add `From` for `FlatDistributions` struct"
type: pull_request
state: merged
author: tdejager
labels:
  - enhancement
assignees: []
merged: true
base: main
head: feat/add-from-flat-distributions
created_at: 2024-11-04T08:27:02Z
updated_at: 2024-11-04T08:41:40Z
url: https://github.com/astral-sh/uv/pull/8800
synced_at: 2026-01-12T16:08:30Z
```

# Add `From` for `FlatDistributions` struct

---

_@tdejager_

I'm using a override `ResolverProvider` and want to make use of the eager version of the `VersionMap`. This way I can construct that variant :)


---

_@konstin reviewed on 2024-11-04 08:34_

---

_Review comment by @konstin on `crates/uv-resolver/src/flat_index.rs`:231 on 2024-11-04 08:34_

```suggestion
/// For external users.
impl From<BTreeMap<Version, PrioritizedDist>> for FlatDistributions {
```

Making sure this doesn't get removed as unused.

---

_@konstin approved on 2024-11-04 08:34_

---

_Label `enhancement` added by @konstin on 2024-11-04 08:34_

---

_Merged by @konstin on 2024-11-04 08:41_

---

_Closed by @konstin on 2024-11-04 08:41_

---
