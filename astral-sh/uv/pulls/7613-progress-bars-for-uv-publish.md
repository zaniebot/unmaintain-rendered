```yaml
number: 7613
title: "Progress bars for `uv publish`"
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/publish5
created_at: 2024-09-21T14:31:30Z
updated_at: 2024-09-24T15:55:35Z
url: https://github.com/astral-sh/uv/pull/7613
synced_at: 2026-01-10T12:53:51Z
```

# Progress bars for `uv publish`

---

_Pull request opened by @konstin on 2024-09-21 14:31_

Add progress bars to `uv publish`, helpful when uploading large packages.

---

_Review requested from @BurntSushi by @konstin on 2024-09-21 14:31_

---

_Label `enhancement` added by @konstin on 2024-09-21 14:31_

---

_Review comment by @BurntSushi on `crates/uv-publish/src/lib.rs`:89 on 2024-09-23 14:24_

How come you went for a trait here instead of asking for a concrete type? An enum would probably do the job in order to handle the case of the "dummy" reporter.

---

_@BurntSushi approved on 2024-09-23 14:25_

LGTM, although I think I would probably try to avoid defining a new trait here in favor of a concrete type or enum. (But I may be missing some context motivating the trait.)

---

_@zanieb reviewed on 2024-09-23 14:30_

---

_Review comment by @zanieb on `crates/uv-publish/src/lib.rs`:89 on 2024-09-23 14:30_

I think this matches our other reporter patterns?

---

_@BurntSushi reviewed on 2024-09-23 14:37_

---

_Review comment by @BurntSushi on `crates/uv-publish/src/lib.rs`:89 on 2024-09-23 14:37_

Ah. That would be a candidate for un-genericizing code in a hope to improve compile times. :)

(It may result in little gain if there's only one instantiation of the generics outside of tests though.)

---

_@konstin reviewed on 2024-09-24 10:01_

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:89 on 2024-09-24 10:01_

It would be nice to have a single reporter crate that is shared (or some callback) instead of copying this code for every new feature.

---

_@BurntSushi reviewed on 2024-09-24 14:52_

---

_Review comment by @BurntSushi on `crates/uv-publish/src/lib.rs`:89 on 2024-09-24 14:52_

Yeah. I think it can be completely monomorphic too with no parametric polymorphism.

---

_Merged by @konstin on 2024-09-24 15:55_

---

_Closed by @konstin on 2024-09-24 15:55_

---

_Branch deleted on 2024-09-24 15:55_

---
