```yaml
number: 398
title: Filter out incompatible dists
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: filter-out-sdists
created_at: 2023-11-10T19:21:26Z
updated_at: 2023-11-13T16:14:08Z
url: https://github.com/astral-sh/uv/pull/398
synced_at: 2026-01-10T15:50:28Z
```

# Filter out incompatible dists

---

_Pull request opened by @konstin on 2023-11-10 19:21_

Filter out source dists and wheels whose `requires-python` from the simple api is incompatible with the current python version.

This change showed an important problem: When we use a fake python version for resolving, building source distributions breaks down because we can only build with versions we actually have.

This change became surprisingly big. The tests now require python 3.7 to be installed, but changing that would mean an even bigger change.

Fixes #388

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/finder.rs`:154 on 2023-11-10 19:28_

I'm surprised this needs to clone -- is it necessary? Should `VersionSpecifiers` from implement `From<&str>`?

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver.rs`:559 on 2023-11-10 19:29_

I think you can omit the issue link here, folks can always look this up in the Git history.

---

_Review comment by @charliermarsh on `crates/pypi-types/src/simple_json.rs`:20 on 2023-11-10 19:31_

I think the type here should be `VersionSpecifiers`, right? As it is in `Metadata21`?

---

_@charliermarsh reviewed on 2023-11-10 19:31_

---

_@charliermarsh reviewed on 2023-11-10 19:55_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/simple_json.rs`:20 on 2023-11-10 19:55_

Like I think the _stored_ type should be `VersionSpecifiers` even if we use `LenientVersionSpecifiers` for parsing...

---

_@charliermarsh reviewed on 2023-11-10 20:57_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_compile.rs`:132 on 2023-11-10 20:57_

Can you say more about this? Don't we want to mirror the _real_ markers in the resolver?

---

_@zanieb reviewed on 2023-11-10 22:28_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/resolver.rs`:557 on 2023-11-10 22:28_

Is this the right place to filter this?

It seems like it'd be ideal of we could reject these versions with a message (in the style of https://github.com/pubgrub-rs/pubgrub/issues/152) so that the reason the versions are rejected are tracked in the derivation tree.

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver.rs`:557 on 2023-11-10 22:33_

Yeah that would be better for sure. Resolvo has this concept too: dependencies that are available but don’t match the current platform. (I don’t see how we can support this right now though, and we already filter out incompatible wheels…)

---

_@charliermarsh reviewed on 2023-11-10 22:33_

---

_@zanieb reviewed on 2023-11-10 22:34_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/resolver.rs`:557 on 2023-11-10 22:34_

Okay — the xref here will be enough for me to revisit if I get this functionality added to PubGrub.

---

_@charliermarsh reviewed on 2023-11-10 23:13_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver.rs`:557 on 2023-11-10 23:13_

Awesome, thank you!

---

_@konstin reviewed on 2023-11-11 12:16_

---

_Review comment by @konstin on `crates/puffin-resolver/src/resolver.rs`:557 on 2023-11-11 12:16_

Something like this, if that works in the pubgrub/resolvo models?

```rust
type VersionMap = BTreeMap<PubGrubVersion, MaybeDistFile>
enum MaybeDistFile {
    /// No compatible wheels and no source distribution (e.g. only an `.exe`)
    NoCompatiblePackage,
    /// No compatible wheels and the source distribution isn't compatible 
    VersionMismatch(VersionSpecifiers),
    ///
    Available {
        /// The highest priority file, usually a wheel
       picked: DistFile,
       /// Other installation candidates, such as lower priority compatible wheels and the source distribution if a wheel exists
        ///
        /// We track them here to add their hashes to the lockfile (the user may e.g. have a different glibc version)
        /// TODO(konstin): Check that this is actually how pip-tools generates the hashes list
       others: Vec<DistFile>
    }
}
```

otoh it might make more sense to keep those sdists in and instead give them a dependency on the python version, so we can properly report a conflict on root requiring python ==3.7, while pandas at the selected version requires python >3.9 

---

_@konstin reviewed on 2023-11-13 10:03_

---

_Review comment by @konstin on `crates/puffin-resolver/src/resolver.rs`:557 on 2023-11-13 10:03_

https://github.com/astral-sh/puffin/issues/406 

---

_@konstin reviewed on 2023-11-13 10:16_

---

_Review comment by @konstin on `crates/puffin-cli/src/commands/pip_compile.rs`:132 on 2023-11-13 10:16_

This is so we use the fake python version everywhere including when recursing for builds, it's effectively https://github.com/astral-sh/puffin/issues/407

---

_Comment by @konstin on 2023-11-13 11:35_

My suggestion would be to merge this because it unblocks a bunch of downstream things where we currently fail on picking an invalid sdist and add the proper #406 solution later

---

_Renamed from "Filter out incompatible source dists" to "Filter out incompatible dists" by @konstin on 2023-11-13 12:18_

---

_Review comment by @konstin on `crates/puffin-cli/tests/snapshots/pip_compile__compile_constraints_markers.snap`:21 on 2023-11-13 13:05_

pip installs this one, too, so i assume it's correct

---

_@konstin reviewed on 2023-11-13 13:05_

---

_@konstin reviewed on 2023-11-13 13:05_

---

_Review comment by @konstin on `crates/puffin-cli/tests/snapshots/pip_compile__compile_constraints_txt.snap`:25 on 2023-11-13 13:05_

pip installs this one, too, so i assume it's correct

---

_@charliermarsh reviewed on 2023-11-13 14:32_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_compile.rs`:132 on 2023-11-13 14:32_

Does this mean we'll end up trying to build source distributions that aren't actually supported? E.g., if we're using Python 3.7, but we use `--python-version py312`, when recursing, would we try to build Python 3.12-only source distributions?

---

_@charliermarsh approved on 2023-11-13 15:37_

---

_Merged by @konstin on 2023-11-13 16:14_

---

_Closed by @konstin on 2023-11-13 16:14_

---

_Branch deleted on 2023-11-13 16:14_

---
