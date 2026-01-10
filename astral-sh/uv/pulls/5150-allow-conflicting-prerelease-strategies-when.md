```yaml
number: 5150
title: Allow conflicting prerelease strategies when forking
type: pull_request
state: merged
author: ibraheemdev
labels:
  - lock
assignees: []
merged: true
base: main
head: ibraheem/fork-prereleases
created_at: 2024-07-17T17:01:59Z
updated_at: 2024-07-23T15:57:17Z
url: https://github.com/astral-sh/uv/pull/5150
synced_at: 2026-01-10T13:37:23Z
```

# Allow conflicting prerelease strategies when forking

---

_Pull request opened by @ibraheemdev on 2024-07-17 17:01_

## Summary

Similar to https://github.com/astral-sh/uv/pull/5232, we should also track prerelease strategies per-fork, instead of globally per package. The common functionality for tracking locals and prerelease versions across forks is extracted into the `ForkMap` type.

Resolves https://github.com/astral-sh/uv/issues/4579. This doesn't quite solve https://github.com/astral-sh/uv/issues/4959, as that issue relies on overlapping markers.


---

_Label `lock` added by @ibraheemdev on 2024-07-17 17:01_

---

_Review requested from @BurntSushi by @ibraheemdev on 2024-07-17 17:01_

---

_Review requested from @konstin by @ibraheemdev on 2024-07-17 17:01_

---

_@ibraheemdev reviewed on 2024-07-17 17:41_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/resolver/mod.rs`:1149 on 2024-07-17 17:41_

Looks like there was a bug in https://github.com/astral-sh/uv/pull/5104 because we only looked for locals on the root package, which ignored editables and path dependencies. We have to set these fields in `PubGrubDependency::from_requirement`, which is a little unfortunate because it means we parse the local from *all* dependencies, even though we only need to look at requirements that are directly in the manifest. Maybe we should move this step up to the `Manifest` directly.

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-07-17 17:57_

---

_@ibraheemdev reviewed on 2024-07-17 20:52_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/resolver/mod.rs`:1149 on 2024-07-17 20:52_

Hmm this might be incorrect, but I'm not sure how to represent what we want here.

---

_Converted to draft by @ibraheemdev on 2024-07-18 02:23_

---

_Review comment by @konstin on `crates/uv-resolver/src/candidate_selector.rs`:117 on 2024-07-18 07:27_

nit: Should we move this block into its own function?

---

_@konstin approved on 2024-07-18 07:31_

---

_Marked ready for review by @ibraheemdev on 2024-07-22 21:52_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/candidate_selector.rs`:129 on 2024-07-23 01:39_

Is this piece correct? I feel like we should still be respecting preferences here.

---

_@charliermarsh reviewed on 2024-07-23 01:39_

---

_@charliermarsh reviewed on 2024-07-23 01:40_

Looks good overall, just one question.

---

_@ibraheemdev reviewed on 2024-07-23 03:03_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/candidate_selector.rs`:129 on 2024-07-23 03:03_

I'm not sure, I think the issue comes from [when we add preferences during resolution](https://github.com/astral-sh/uv/blob/main/crates/uv-resolver/src/resolver/mod.rs#L384), because we can end up adding a preference for a prerelease from a different fork. Maybe the solution is to make `Preferences` a `ForkMap`?

---

_@ibraheemdev reviewed on 2024-07-23 03:07_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/candidate_selector.rs`:129 on 2024-07-23 03:07_

Actually I feel like this is correct. If pre-release versions are not allowed then the version range is not actually satisfied, right?

---

_@charliermarsh approved on 2024-07-23 15:05_

---

_Merged by @ibraheemdev on 2024-07-23 15:57_

---

_Closed by @ibraheemdev on 2024-07-23 15:57_

---

_Branch deleted on 2024-07-23 15:57_

---
