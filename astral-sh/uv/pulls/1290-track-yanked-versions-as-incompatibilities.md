```yaml
number: 1290
title: Track yanked versions as incompatibilities
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/yanked-incompatibility
created_at: 2024-02-12T21:49:29Z
updated_at: 2024-02-13T04:02:29Z
url: https://github.com/astral-sh/uv/pull/1290
synced_at: 2026-01-12T16:04:34Z
```

# Track yanked versions as incompatibilities

---

_@zanieb_

Moves yanked version filtering from `VersionMap::from_metadata` to the resolver and tracks it as a PubGrub unavailable incompatibility so yanked versions are reflected in error messages.

e.g. before
```
╰─▶ Because only albatross<=0.1.0 is available and you require albatross>0.1.0, 
       we can conclude that the requirements are unsatisfiable.
```

after

```
╰─▶ Because only the following versions of albatross are available:
            albatross<=0.1.0
            albatross==1.0.0
      and albatross==1.0.0 is unusable because it was yanked, we can conclude that albatross>0.1.0 cannot be used.
      And because you require albatross>0.1.0, we can conclude that the requirements are unsatisfiable.
```

---

_@zanieb reviewed on 2024-02-12 23:23_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/package.rs`:110 on 2024-02-12 23:23_

I think this is basically wrong e.g. if we have a package with a URL it would not have this format.

It's hard to index into a map with the package id consistently in the resolver. We probably should address it there somehow instead of doing this.

---

_@zanieb reviewed on 2024-02-12 23:30_

---

_Review comment by @zanieb on `crates/distribution-types/src/prioritized_distribution.rs`:230 on 2024-02-12 23:30_

I want to refactor this `DistRequiresPython` thing but will probably try to do so in a separate pull request

---

_@zanieb reviewed on 2024-02-12 23:31_

---

_Review comment by @zanieb on `crates/distribution-types/src/prioritized_distribution.rs`:216 on 2024-02-12 23:31_

We probably want a container type for `Yanked` but perhaps that should be a follow-up refactor.

---

_@zanieb reviewed on 2024-02-12 23:32_

---

_Review comment by @zanieb on `crates/puffin-client/src/flat_index.rs`:254 on 2024-02-12 23:32_

Now that there's a default implementation for `Yanked` I should probably use that instead here

---

_@zanieb reviewed on 2024-02-12 23:32_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/resolver/mod.rs`:388 on 2024-02-12 23:32_

This is _only_ used for safeguards in the `get_dependencies` method now. Perhaps we should remove it entirely.

---

_@zanieb reviewed on 2024-02-12 23:34_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/resolver/mod.rs`:421 on 2024-02-12 23:34_

This is a little awkward, this is a combination of the special `get_dependencies` implementation and the partial solution upkeep below. I think we can do better but this is tough.

---

_@zanieb reviewed on 2024-02-12 23:35_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/resolver/mod.rs`:545 on 2024-02-12 23:35_

This allows us to return the incompatible version directly instead of putting it in a map then immediately pulling it from the map in `solve`. It's hard to pull from the map in `solve` because we do not have a `candidate` to get package ids from. Lots of ways to solve that, but this seemed reasonable.

---

_@charliermarsh reviewed on 2024-02-12 23:45_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/version_map.rs`:83 on 2024-02-12 23:45_

Is it true that this allowed per the spec, or does the data model just suggest that it's possible?

---

_@charliermarsh reviewed on 2024-02-12 23:46_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/pubgrub/package.rs`:110 on 2024-02-12 23:46_

Where is this being used?

---

_@zanieb reviewed on 2024-02-13 00:41_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/package.rs`:110 on 2024-02-13 00:41_

In `solve` when we insert items into the `unavailable_versions` mapping.

---

_Review comment by @zanieb on `crates/puffin-resolver/src/version_map.rs`:83 on 2024-02-13 00:44_

Uhh it's an implementation detail 

https://peps.python.org/pep-0592/#warehouse-pypi-implementation-notes

> While this PEP implements yanking at the file level, that is largely due to the shape the simple repository API takes, not a specific decision made by this PEP.
>
> In Warehouse, the user experience will be implemented in terms of yanking or unyanking an entire release, rather than as an operation on individual files, which will then be exposed via the API as individual files being yanked.
> 
> Other repository implementations may choose to expose this capability in a different way, or not expose it at all.

---

_@zanieb reviewed on 2024-02-13 00:44_

---

_@zanieb reviewed on 2024-02-13 00:44_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/version_map.rs`:83 on 2024-02-13 00:44_

Should we implement it per-file? I think it's doable that way too?

---

_@charliermarsh reviewed on 2024-02-13 00:58_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:388 on 2024-02-13 00:58_

I would probably vote to remove since it's also creating that oddity with the `package_id` API.

---

_@zanieb reviewed on 2024-02-13 01:20_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/resolver/mod.rs`:388 on 2024-02-13 01:20_

I'm worried we'll need these for error message tips, but I can probably find a way around that if so.

---

_@zanieb reviewed on 2024-02-13 01:38_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/resolver/mod.rs`:421 on 2024-02-13 01:38_

Cleaned up.

---

_@zanieb reviewed on 2024-02-13 02:12_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/version_map.rs`:83 on 2024-02-13 02:12_

I'm going to tackle this as part of the refactor of `DistRequiresPython`

---

_Marked ready for review by @zanieb on 2024-02-13 02:12_

---

_@zanieb reviewed on 2024-02-13 02:16_

---

_Review comment by @zanieb on `crates/distribution-types/src/prioritized_distribution.rs`:216 on 2024-02-13 02:16_

https://github.com/astral-sh/puffin/pull/1291

---

_@zanieb reviewed on 2024-02-13 02:16_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/version_map.rs`:83 on 2024-02-13 02:16_

https://github.com/astral-sh/puffin/pull/1291

---

_@zanieb reviewed on 2024-02-13 02:16_

---

_Review comment by @zanieb on `crates/distribution-types/src/prioritized_distribution.rs`:230 on 2024-02-13 02:16_

https://github.com/astral-sh/puffin/pull/1291

---

_Review comment by @charliermarsh on `crates/distribution-types/src/prioritized_distribution.rs`:27 on 2024-02-13 02:36_

`?`

---

_@charliermarsh approved on 2024-02-13 02:36_

Excellent.

---

_@charliermarsh reviewed on 2024-02-13 02:37_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/version_map.rs`:83 on 2024-02-13 02:37_

(FWIW I'm fine with it being tracked at a version level given the above, but I also assumed it would add a lot of complexity to track it at a file level.)

---

_Label `error messages` added by @zanieb on 2024-02-13 03:09_

---

_@zanieb reviewed on 2024-02-13 03:10_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/version_map.rs`:83 on 2024-02-13 03:10_

👍 it ended up being cleaner at a file level anyway so I'm just going to go with that

---

_Merged by @zanieb on 2024-02-13 04:01_

---

_Closed by @zanieb on 2024-02-13 04:01_

---

_Branch deleted on 2024-02-13 04:01_

---

_@zanieb reviewed on 2024-02-13 04:02_

---

_Review comment by @zanieb on `crates/distribution-types/src/prioritized_distribution.rs`:27 on 2024-02-13 04:02_

Resolved in #1291 already

---
