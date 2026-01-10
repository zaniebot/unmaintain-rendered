```yaml
number: 581
title: Modify install plan to support all distribution types
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/install-plan
created_at: 2023-12-06T20:28:03Z
updated_at: 2023-12-07T04:44:57Z
url: https://github.com/astral-sh/uv/pull/581
synced_at: 2026-01-10T15:44:44Z
```

# Modify install plan to support all distribution types

---

_Pull request opened by @charliermarsh on 2023-12-06 20:28_

This PR adds caching support for built wheels in the installer. Specifically, the `RegistryWheelIndex` now indexes both downloaded and built wheels (from registries), and we have a new `BuiltWheelIndex` that takes a subdirectory and returns the "best-matching" compatible wheel.

Closes #570.


---

_Review requested from @konstin by @charliermarsh on 2023-12-06 20:28_

---

_@konstin reviewed on 2023-12-06 21:49_

---

_Review comment by @konstin on `crates/puffin-cli/tests/pip_sync.rs`:1288 on 2023-12-06 21:49_

But that's installing a wheel, not a built source dist, isn't it?

---

_Review comment by @konstin on `crates/puffin-installer/src/index/built_wheel_index.rs`:19 on 2023-12-06 21:51_

Could you a note what `directory` looks like / points to? Alternatively we can pass in `Cache`, `WheelCache` and `SourceDistFilename` for it to be self documenting.

---

_Review comment by @konstin on `crates/puffin-installer/src/index/built_wheel_index.rs`:31 on 2023-12-06 21:52_

Do you want to add more functions to this struct? Right now i'd make it a static function with two arguments (or four, when passing in the cache path parts) instead

---

_Review comment by @konstin on `crates/puffin-installer/src/index/mod.rs`:12 on 2023-12-06 21:53_

What about moving this to `puffin-fs` or `puffin-cache`? The whole module might fit better in `puffin-cache`

---

_Review comment by @konstin on `crates/puffin-installer/src/index/built_wheel_index.rs`:53 on 2023-12-06 21:57_

Shouldn't they all be guaranteed to have the same version? Theoretically the source dist could `randint()` the version at build time, but i don't think we should support that, it breaks our assumption that all wheels of the same version have the same metadata 

---

_@charliermarsh reviewed on 2023-12-06 21:59_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/pip_sync.rs`:1288 on 2023-12-06 21:59_

Correct! We already have a separate test for direct URL wheels. (I forgot to add this variant in the earlier PR.)

---

_Review requested from @konstin by @charliermarsh on 2023-12-06 22:05_

---

_Label `enhancement` added by @charliermarsh on 2023-12-06 22:05_

---

_Review comment by @konstin on `crates/puffin-installer/src/index/registry_wheel_index.rs`:45 on 2023-12-06 22:17_

Could you rename this to make it clearer that `subdir` is a source dist (if i read that correctly)?

---

_Review comment by @konstin on `crates/puffin-installer/src/index/registry_wheel_index.rs`:63 on 2023-12-06 22:19_

```suggestion
    /// Add the wheels in a given directory to the index.
    /// 
    /// Each subdirectory in the given path is expected to be an unzipped wheel directory.
```

---

_Review comment by @konstin on `crates/puffin-installer/src/index/registry_wheel_index.rs`:21 on 2023-12-06 22:22_

Maybe `RegistryDistIndex` instead? I want to make it clear that this is both wheels and built wheels.

---

_Review comment by @konstin on `crates/puffin-installer/src/plan.rs`:123 on 2023-12-06 22:45_

```suggestion
                            // Find the exact wheel from the cache, since we know the wheel
```

---

_Review comment by @konstin on `crates/puffin-installer/src/plan.rs`:144 on 2023-12-06 22:45_

```suggestion
                            // Find the exact wheel from the cache, since we know the wheel
```

---

_Review comment by @konstin on `crates/puffin-installer/src/plan.rs`:166 on 2023-12-06 22:46_

```suggestion
                            // Find the most-compatible built wheel from the cache, since we
                            // can't tell the built wheel filename from the source dist filename.
```

---

_@konstin reviewed on 2023-12-06 22:47_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/index/mod.rs`:12 on 2023-12-06 23:13_

How about in `puffin-distribution`, alongside the database?

---

_@charliermarsh reviewed on 2023-12-06 23:13_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/index/built_wheel_index.rs`:53 on 2023-12-06 23:14_

Hmm, what if the URL is like `https://google.com/foo.tar.gz`? The built wheel version could change over time, couldn't it?

---

_@charliermarsh reviewed on 2023-12-06 23:14_

---

_@charliermarsh reviewed on 2023-12-07 04:35_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/index/built_wheel_index.rs`:19 on 2023-12-07 04:35_

This gets changed to take `CacheShard` in the next PR. I will document it further but won't change the type for now.

---

_@charliermarsh reviewed on 2023-12-07 04:39_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/index/built_wheel_index.rs`:31 on 2023-12-07 04:39_

Going to change after #583.

---

_Merged by @charliermarsh on 2023-12-07 04:43_

---

_Closed by @charliermarsh on 2023-12-07 04:43_

---

_Branch deleted on 2023-12-07 04:43_

---

_Comment by @charliermarsh on 2023-12-07 04:44_

(Not going rogue, Konsti gave me the sign-off to merge once feedback was addressed.)

---
