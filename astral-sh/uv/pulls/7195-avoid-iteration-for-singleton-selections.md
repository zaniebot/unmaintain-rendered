```yaml
number: 7195
title: Avoid iteration for singleton selections
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/d
created_at: 2024-09-08T18:04:06Z
updated_at: 2024-09-09T13:43:58Z
url: https://github.com/astral-sh/uv/pull/7195
synced_at: 2026-01-10T12:53:42Z
```

# Avoid iteration for singleton selections

---

_Pull request opened by @charliermarsh on 2024-09-08 18:04_

## Summary

If we have a singleton `Range`, we don't need to iterate over the map of available ranges; instead, we can just get the singleton directly.

Closes #6131.


---

_Label `performance` added by @charliermarsh on 2024-09-08 18:04_

---

_Converted to draft by @charliermarsh on 2024-09-08 18:10_

---

_Comment by @charliermarsh on 2024-09-08 18:15_

Most of the test failures are just aesthetic, but there is one real failure due to this:

```rust
// If we have at least one stable release, we shouldn't allow the "if-necessary"
// pre-release strategy, regardless of whether that stable release satisfies the
// current range.
prerelease = Some(PrereleaseCandidate::NotNecessary);
```

So the selector actually _does_ rely on seeing versions outside the range, hmm.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/candidate_selector.rs`:342 on 2024-09-08 23:45_

A little bit of a penalty here but should be somewhat rare.

---

_@charliermarsh reviewed on 2024-09-08 23:45_

---

_Marked ready for review by @charliermarsh on 2024-09-09 00:05_

---

_Review requested from @konstin by @charliermarsh on 2024-09-09 00:05_

---

_@charliermarsh reviewed on 2024-09-09 00:06_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/candidate_selector.rs`:342 on 2024-09-09 00:06_

We only need to do this slow path when we see a pre-release singleton; and the check will short-circuit as soon as we see any non-pre-release.

---

_@charliermarsh reviewed on 2024-09-09 00:06_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/candidate_selector.rs`:342 on 2024-09-09 00:06_

It'd be nice to avoid this though.

---

_Review comment by @konstin on `crates/uv-resolver/src/candidate_selector.rs`:342 on 2024-09-09 03:49_

You could store whether all versions are prereleases in the version map, probably helpful for debugging anyway

---

_Review comment by @konstin on `crates/uv-resolver/src/version_map.rs`:157 on 2024-09-09 03:53_

```suggestion
        // Performance optimization: If we only have a single version, return that version directly.
        if let Some(version) = range.as_singleton() {
```

Without a comment, i can already see myself looking at this code later and wondering whether the branch is load-bearing or not.

---

_@konstin approved on 2024-09-09 03:55_

---

_@charliermarsh reviewed on 2024-09-09 13:21_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/candidate_selector.rs`:342 on 2024-09-09 13:21_

Good idea.

---

_Merged by @charliermarsh on 2024-09-09 13:43_

---

_Closed by @charliermarsh on 2024-09-09 13:43_

---

_Branch deleted on 2024-09-09 13:43_

---
