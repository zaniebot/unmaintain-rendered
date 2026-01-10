```yaml
number: 4114
title: "Rename `Dependency.id` to `Dependency.distribution_id`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/dep-id
created_at: 2024-06-06T20:15:10Z
updated_at: 2024-06-07T18:28:55Z
url: https://github.com/astral-sh/uv/pull/4114
synced_at: 2026-01-10T13:54:02Z
```

# Rename `Dependency.id` to `Dependency.distribution_id`

---

_Pull request opened by @charliermarsh on 2024-06-06 20:15_

## Summary

I think this makes clearer that the `Dependency.id` is not an identifier for the dependency itself.

No functional changes.


---

_Label `internal` added by @charliermarsh on 2024-06-06 20:15_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-06-07 00:03_

---

_@charliermarsh reviewed on 2024-06-07 00:03_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:762 on 2024-06-07 00:03_

This is closer to the standard syntax we use for dependencies.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:1467 on 2024-06-07 17:54_

When I see `DependencyId`, I think, "what is it an ID for? what is it identifying?" For a `DistributionId`, it's used to lookup a `Distribution`. What can a `DependencyId` lookup?

---

_@BurntSushi reviewed on 2024-06-07 17:55_

---

_@charliermarsh reviewed on 2024-06-07 18:08_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:1467 on 2024-06-07 18:08_

Ok. I can change it back to `Dependency`, but I don't think the field on it should be called `id`, since that's not an ID for the _dependency_.

---

_@BurntSushi reviewed on 2024-06-07 18:12_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:1467 on 2024-06-07 18:12_

Yeah that's fair.

---

_Renamed from "Reframe `Dependency` struct as `DependencyId`" to "Rename `Dependency.id` to `Dependency.distribution_id`" by @charliermarsh on 2024-06-07 18:21_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-06-07 18:22_

---

_@BurntSushi approved on 2024-06-07 18:26_

---

_Merged by @charliermarsh on 2024-06-07 18:28_

---

_Closed by @charliermarsh on 2024-06-07 18:28_

---

_Branch deleted on 2024-06-07 18:28_

---
