```yaml
number: 12281
title: "Make cache errors non-fatal in Planner::build"
type: pull_request
state: merged
author: Gankra
labels:
  - bug
assignees: []
merged: true
base: main
head: gankra/cache-err
created_at: 2025-03-18T14:18:39Z
updated_at: 2025-03-18T15:27:23Z
url: https://github.com/astral-sh/uv/pull/12281
synced_at: 2026-01-12T16:10:12Z
```

# Make cache errors non-fatal in Planner::build

---

_@Gankra_

Same basic approach as #11105, including a cache version bump.

Fixes #12274

---

_Label `bug` added by @Gankra on 2025-03-18 14:18_

---

_@charliermarsh reviewed on 2025-03-18 14:20_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/plan.rs`:112 on 2025-03-18 14:20_

We might want this to be `warn!`

---

_@charliermarsh approved on 2025-03-18 14:20_

---

_@charliermarsh reviewed on 2025-03-18 14:22_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/plan.rs`:112 on 2025-03-18 14:22_

I guess this can fail for other reasons though. Should it be more discriminant or no?

---

_@charliermarsh reviewed on 2025-03-18 14:23_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/plan.rs`:270 on 2025-03-18 14:23_

Nit: caps

---

_@Gankra reviewed on 2025-03-18 14:27_

---

_Review comment by @Gankra on `crates/uv-installer/src/plan.rs`:112 on 2025-03-18 14:27_

At least in the current implementation, it actually is all CacheInfo calls that make this fallible. warn feels too aggressive for "oops the cache was invalid"?

---

_@charliermarsh reviewed on 2025-03-18 14:35_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/plan.rs`:112 on 2025-03-18 14:35_

I feel like we should always know if we hit this though, it's almost always a sign of a bug. I'm honestly wondering if this should fail hard in debug?

---

_@charliermarsh reviewed on 2025-03-18 14:35_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/plan.rs`:112 on 2025-03-18 14:35_

But that's probably out-of-scope.

---

_@charliermarsh reviewed on 2025-03-18 14:36_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/plan.rs`:112 on 2025-03-18 14:36_

Should we move this into `RequirementSatisfaction::check`? If it's already non-fatal for other kinds of errors, maybe it shouldn't return `Result` at all and we should just ignore these errors within that method.

---

_Review comment by @Gankra on `crates/uv-installer/src/plan.rs`:112 on 2025-03-18 14:48_

That sounds like a good idea... hmm interesting SitePackages::satisfies_requirements also `?`s this method and it probably shouldn't..?

---

_@Gankra reviewed on 2025-03-18 14:48_

---

_@Gankra reviewed on 2025-03-18 15:00_

---

_Review comment by @Gankra on `crates/uv-installer/src/plan.rs`:112 on 2025-03-18 15:00_

Implemented as new commit.

---

_Merged by @Gankra on 2025-03-18 15:27_

---

_Closed by @Gankra on 2025-03-18 15:27_

---

_Branch deleted on 2025-03-18 15:27_

---
