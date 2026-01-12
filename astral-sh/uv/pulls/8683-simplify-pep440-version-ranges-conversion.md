```yaml
number: 8683
title: "Simplify pep440 -> version ranges conversion"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/version-ranges-3
created_at: 2024-10-29T20:26:49Z
updated_at: 2024-10-30T12:10:50Z
url: https://github.com/astral-sh/uv/pull/8683
synced_at: 2026-01-12T16:08:26Z
```

# Simplify pep440 -> version ranges conversion

---

_@konstin_

With #8669 as preparation, we can remove what used to be the `PubGrubSpecifier` and instead implement a direct conversion from `VersionSpecifiers` to `Range<Version>`. Thanks to https://github.com/astral-sh/uv/pull/8669#discussion_r1821241638, this conversion is infallible, removing a lot of fallibility downstream.

The conversion takes an owned value, to signal that we consume the value. Previously, the function had cloned the version internally.


---

_Label `internal` added by @konstin on 2024-10-29 20:26_

---

_Review requested from @BurntSushi by @konstin on 2024-10-29 20:26_

---

_Review comment by @BurntSushi on `crates/uv-pep440/src/version_ranges.rs`:13 on 2024-10-30 11:59_

At first, I thought this might have been allocating a new `Ranges<Version>` only to throw it away, but I think `Ranges::from(specifier)` in this case always creates a `Ranges<Version>` with one range? (Possibly two?) If so, and I believe since it uses smallvec internally, there's no alloc happening.

I don't have any suggestions for the immediate term, but if this ever showed up on a profile, there might be some room for micro-optimization here. (Perhaps requiring API changes to `version-ranges`. Not sure.)

---

_Review comment by @BurntSushi on `crates/uv-pep440/src/version_ranges.rs`:13 on 2024-10-30 11:59_

Also guessing that this is mostly moved code and not newly written.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/requires_python/tests.rs`:14 on 2024-10-30 12:01_

Nice!

---

_Review comment by @BurntSushi on `crates/uv/src/commands/project/init.rs`:365 on 2024-10-30 12:02_

Nice!

---

_@BurntSushi approved on 2024-10-30 12:02_

I like it!

---

_Review comment by @konstin on `crates/uv-pep440/src/version_ranges.rs`:13 on 2024-10-30 12:10_

Agreed, keeping it since it's the same overhead as it was before the refactoring.

---

_@konstin reviewed on 2024-10-30 12:10_

---

_Merged by @konstin on 2024-10-30 12:10_

---

_Closed by @konstin on 2024-10-30 12:10_

---

_Branch deleted on 2024-10-30 12:10_

---
