```yaml
number: 14271
title: "Fix equals-star and tilde-equals with `python_version` and `python_full_version`"
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/fix-star-inequality-trailing-zeroes
created_at: 2025-06-26T09:56:35Z
updated_at: 2025-07-01T15:48:50Z
url: https://github.com/astral-sh/uv/pull/14271
synced_at: 2026-01-12T16:11:07Z
```

# Fix equals-star and tilde-equals with `python_version` and `python_full_version`

---

_@konstin_

The marker display code assumes that all versions are normalized, in that all trailing zeroes are stripped. This is not the case for tilde-equals and equals-star versions, where the trailing zeroes (before the `.*`) are semantically relevant. This would cause path dependent-behavior where we would get a different marker string depending on whether a version with or without a trailing zero was added to the cache first.

To handle both equals-star and tilde-equals when converting `python_version` to `python_full_version` markers, we have to merge the version normalization (i.e. trimming the trailing zeroes) and the conversion both to `python_full_version` and to `Ranges`, while special casing equals-star and tilde-equals.

To avoid churn in lockfiles, we only trim in the conversion to `Ranges` for markers, but keep using untrimmed versions for requires-python. (Note that this behavior is technically also path dependent, as versions with and without trailing zeroes have the same Hash and Eq. E.q., `requires-python == ">= 3.10.0"` and  `requires-python == ">= 3.10"` in the same workspace could lead to either value in `uv.lock`, and which one it is could change if we make unrelated (performance) changes. Always trimming however definitely changes lockfiles, a churn I wouldn't do outside another breaking or lockfile-changing change.) Nevertheless, there is a change for users who have `requires-python = "~= 3.12.0"` in their `pyproject.toml`, as this now hits the correct normalization path.

Fixes #14231
Fixes #14270

---

_Review requested from @ibraheemdev by @konstin on 2025-06-26 09:56_

---

_Label `bug` added by @konstin on 2025-06-26 09:56_

---

_Review comment by @ibraheemdev on `crates/uv-pep508/src/marker/simplify.rs`:356 on 2025-06-26 20:11_

Could this logic be added to `VersionSpecifier::from_release_only_bounds` instead? This whole function is just a special case of `VersionSpecifiers::from_release_only_bounds` for the case where we have exactly two ranges that form a not-equals-star. Otherwise we call `from_release_only_bounds` for the individual range bounds.

---

_@ibraheemdev reviewed on 2025-06-26 20:11_

---

_Renamed from "Fix a case where marker formatting is dependent on operation order" to "Fix equals-star and tilde-equals with `python_version` and `python_full_version`" by @konstin on 2025-06-30 08:19_

---

_@konstin reviewed on 2025-06-30 08:32_

---

_Review comment by @konstin on `crates/uv-pep508/src/marker/tree.rs`:2276 on 2025-06-30 08:32_

Like I'm fine if we drop support for them in a future refactoring, they are dubious to begin with.

---

_Review comment by @konstin on `crates/uv-pep508/src/marker/tree.rs`:2576 on 2025-06-30 08:34_

This does not yet include the `test_tilde_equal_inverted_normalization` test from #14259.

---

_@konstin reviewed on 2025-06-30 08:34_

---

_Review comment by @konstin on `crates/uv-pep508/src/marker/simplify.rs`:356 on 2025-06-30 09:04_

How'd we do that? Currently, we call `VersionSpecifier::from_release_only_bounds` in a loop, but only with a single bound.

---

_@konstin reviewed on 2025-06-30 09:04_

---

_Review requested from @ibraheemdev by @konstin on 2025-06-30 09:23_

---

_@konstin reviewed on 2025-06-30 10:00_

---

_Review comment by @konstin on `crates/uv-pep440/src/version_specifier.rs`:82 on 2025-06-30 10:00_

This function seems to have only integration test coverage, do we have good unit test cases?

---

_Review comment by @ibraheemdev on `crates/uv-pep508/src/marker/simplify.rs`:68 on 2025-06-30 23:52_

I wonder if we should just be using `VersionSpecifiers::from_release_only_bounds` here instead of specifying the case with two bounds. It could possibly lead to more simplifications for complex ranges (e.g. 3 ranges where 2 of them can be simplified to a star inequality)? It would also mean de-duplicating the code and getting better test coverage.

---

_@ibraheemdev approved on 2025-06-30 23:54_

This looks great and should be a lot more robust. Maybe we should open an issue for the `requires-python` change, because having both behaviors might lead to issues down the road.

---

_@konstin reviewed on 2025-07-01 08:40_

---

_Review comment by @konstin on `crates/uv-pep508/src/marker/simplify.rs`:68 on 2025-07-01 08:40_

I agree with merging those for being very similar, though I'm struggling to understand how `VersionSpecifiers::from_release_only_bounds` works, it seems that it drops some specifiers while `simplify.rs` doesn't?

---

_@konstin reviewed on 2025-07-01 15:48_

---

_Review comment by @konstin on `crates/uv-pep508/src/marker/simplify.rs`:68 on 2025-07-01 15:48_

I'll merge this for the release but I can follow up improving the code here

---

_Merged by @konstin on 2025-07-01 15:48_

---

_Closed by @konstin on 2025-07-01 15:48_

---

_Branch deleted on 2025-07-01 15:48_

---
