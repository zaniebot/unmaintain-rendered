```yaml
number: 278
title: Wheel filename distribution package name
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: WheelFilename-distribution-package-name
created_at: 2023-11-01T16:38:42Z
updated_at: 2023-11-02T11:15:28Z
url: https://github.com/astral-sh/uv/pull/278
synced_at: 2026-01-12T16:03:50Z
```

# Wheel filename distribution package name

---

_@konstin_

The normalized name abstractions were not consistently, this PR uses them where they were previously missing:
* `WheelFilename::distribution`
* `Requirement::name`
* `Requirement::extras`
* `Metadata21::name`
* `Metadata21::provides_dist`

With `puffin-package` depending on `pep508_rs` this would be cyclical crate dependency, so `puffin-normalize` gets split out from `puffin-package`.

`DistInfoName` has the same task and semantics as `PackageName`, so it's merged into the latter.

`PackageName` and `ExtraName` documentation is moved onto the type and their constructors are called `new` instead of `normalize`. We now use these constructors rarely enough the implicit allocation by `to_string()` shouldn't matter anymore, while more actual cloning becomes visible.

---

_Comment by @zanieb on 2023-11-01 17:09_

Perhaps relevant #279 

---

_Comment by @konstin on 2023-11-01 17:11_

I'd rebase #279 on top of this. We can then replace the default serde impl with one that validates and make `PackageName::new` fallible as it should be

---

_Marked ready for review by @konstin on 2023-11-01 17:11_

---

_@charliermarsh approved on 2023-11-01 23:36_

---

_Merged by @konstin on 2023-11-02 11:15_

---

_Closed by @konstin on 2023-11-02 11:15_

---

_Branch deleted on 2023-11-02 11:15_

---
