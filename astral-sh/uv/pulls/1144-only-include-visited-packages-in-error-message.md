```yaml
number: 1144
title: Only include visited packages in error message derivation
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/visits
created_at: 2024-01-27T01:49:32Z
updated_at: 2024-01-28T14:27:23Z
url: https://github.com/astral-sh/uv/pull/1144
synced_at: 2026-01-12T16:04:28Z
```

# Only include visited packages in error message derivation

---

_@charliermarsh_

## Summary

This is my guess as to the source of the resolver flake, based on information and extensive debugging from @zanieb. In short, if we rely on `self.index.packages` as a source of truth during error reporting, we open ourselves up to a source of non-determinism, because we fetch package metadata asynchronously in the background while we solve -- so packages _could_ be included in or excluded from the index depending on the order in which those requests are returned.

So, instead, we now track the set of packages that _were_ visited by the solver. Visiting a package _requires_ that we wait for its metadata to be available. By limiting analysis to those packages that were visited during solving, we are faithfully representing the state of the solver at the time of failure.

Closes #863 

---

_Review requested from @zanieb by @charliermarsh on 2024-01-27 01:49_

---

_Label `bug` added by @charliermarsh on 2024-01-27 01:49_

---

_@charliermarsh reviewed on 2024-01-27 01:50_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/error.rs`:174 on 2024-01-27 01:50_

@zanieb - I don't think it's necessary true that we'll always have metadata for all packages in the derivation tree (`for package in self.derivation_tree.packages()`), since the derivation tree can include _dependencies_ that we haven't yet visited.

---

_@charliermarsh reviewed on 2024-01-27 01:50_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/error.rs`:172 on 2024-01-27 01:50_

We could "join" `visited` and `package_versions` prior to passing it in here, if we want.

---

_@zanieb reviewed on 2024-01-27 03:07_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/error.rs`:195 on 2024-01-27 03:07_

We need metadata for every package in the derivation tree or we will not be able to simplify ranges when displaying errors regarding that package. This change will degrade error messages; that's why I'm suggesting that we attempt to retrieve the metadata of any packages in the derivation tree that are not available. I can take a shot at implementing it.

If we wanted to use the approach implemented here, we could just skip simplifying if the iterable is empty in `simplify_set` instead of doing this extra tracking in the resolver. Packages with no versions are represented by a different incompatibility anyway.

---

_@charliermarsh reviewed on 2024-01-27 03:48_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/error.rs`:195 on 2024-01-27 03:48_

That feels wrong from a correctness perspective. Doesn't it mean we're displaying error messages that don't represent the current state of the solver?

---

_@charliermarsh reviewed on 2024-01-27 03:53_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/error.rs`:195 on 2024-01-27 03:53_

> If we wanted to use the approach implemented here, we could just skip simplifying if the iterable is empty in `simplify_set` instead of doing this extra tracking in the resolver. Packages with no versions are represented by a different incompatibility anyway.

This part I don't understand (the iterable is _not_ empty today, which is the source of the flake, right? We have no idea if there are no versions of the package that fails this test; we just don't know what the versions are yet) but I'll defer to you.


---

_@zanieb reviewed on 2024-01-27 18:34_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/error.rs`:195 on 2024-01-27 18:34_

To clarify the current situation, most of the time the iterable is not empty so we say that `... depends on bluebird <ranges>` and "simplify" the ranges to cover the full range of versions of `bluebird` available. When the iterable is empty, simplification fails and we enumerate _all_ of the unsimplified ranges of  `bluebird` that the solver cares about.

If package is present in the derivation tree, we _are_ going to reference it in the error message. The question is whether or not we will ever need to simplify the ranges associated with that package. I'm struggling to come up with a scenario where this is essential, i.e. where the unsimplified ranges of `bluebird` are excessively verbose.

I think we should just forward with what you have here until we see real world evidence that simplifying ranges for packages that the solver did not visit is important.

---

_Marked ready for review by @zanieb on 2024-01-27 18:34_

---

_@zanieb approved on 2024-01-27 18:35_

Thanks!

---

_@zanieb reviewed on 2024-01-27 18:35_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/error.rs`:172 on 2024-01-27 18:35_

This seems clear enough.

---

_@charliermarsh reviewed on 2024-01-28 03:16_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/error.rs`:195 on 2024-01-28 03:16_

Ok sounds god -- I will clean this up and merge. Assuming it works, you get all the credit. If it doesn't, I will take all the blame.

---

_Merged by @charliermarsh on 2024-01-28 14:27_

---

_Closed by @charliermarsh on 2024-01-28 14:27_

---

_Branch deleted on 2024-01-28 14:27_

---
