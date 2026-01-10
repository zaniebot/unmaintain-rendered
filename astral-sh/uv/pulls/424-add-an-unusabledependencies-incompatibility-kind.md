```yaml
number: 424
title: "Add an `UnusableDependencies` incompatibility kind and use for conflicting versions"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/new-incompatibility
created_at: 2023-11-14T16:48:34Z
updated_at: 2023-11-16T20:02:08Z
url: https://github.com/astral-sh/uv/pull/424
synced_at: 2026-01-10T15:50:28Z
```

# Add an `UnusableDependencies` incompatibility kind and use for conflicting versions

---

_Pull request opened by @zanieb on 2023-11-14 16:48_

Addresses https://github.com/astral-sh/puffin/issues/309#issuecomment-1792648969

Similar to #338 this throws an error when merging versions results in an empty set. Instead of propagating that error, we capture it and return a new dependency type of `Unusable`. Unusable dependencies are a new incompatibility kind which includes an arbitrary "reason" string that we present to the user. Adding a new incompatibility kind requires changes to the vendored pubgrub crate.

We could use this same incompatibility kind for conflicting urls as in #284 which should allow the solver to backtrack to another valid version instead of failing (see #425).

Unlike #383 this does not require changes to PubGrub's package mapping model. I think in the long run we'll want PubGrub to accept multiple versions per package to solve this specific issue, but we're interested in it being merged upstream first. This pull request is just using the issue as a simple case to explore adding a new incompatibility type. 

We may or may not be able convince them to add this new incompatibility type upstream. As discussed in https://github.com/pubgrub-rs/pubgrub/issues/152, we may want a more general incompatibility kind instead which can be used for arbitrary problems. An upstream pull request has been opened for discussion at https://github.com/pubgrub-rs/pubgrub/pull/153.

Related to:
- https://github.com/pubgrub-rs/pubgrub/issues/152
- #338 
- #383 

---

_@zanieb reviewed on 2023-11-14 16:57_

---

_Review comment by @zanieb on `vendor/pubgrub/src/report.rs`:122 on 2023-11-14 16:57_

👀 this is probably not quite right because we shouldn't merge the reasons here

---

_@charliermarsh reviewed on 2023-11-14 17:18_

---

_Review comment by @charliermarsh on `vendor/pubgrub/src/report.rs`:122 on 2023-11-14 17:18_

Can you say a bit more about this?

---

_@zanieb reviewed on 2023-11-14 17:24_

---

_Review comment by @zanieb on `vendor/pubgrub/src/report.rs`:122 on 2023-11-14 17:24_

I'm not sure it matters, but this merges the two incompatibilities by taking the union of their version ranges and _one_ of the reasons but if the reasons are different we should not merge them because they are distinct incompatibilities. Or.. we could merge them and say "{reason} and {reason}" but that seems unclear later on since you lose the reason for each range.

---

_Review comment by @charliermarsh on `vendor/pubgrub/src/report.rs`:192 on 2023-11-14 17:24_

This seems generic enough that it could feasibly be upstreamed 🤔 

---

_@charliermarsh reviewed on 2023-11-14 17:24_

---

_@charliermarsh reviewed on 2023-11-14 17:26_

---

_Review comment by @charliermarsh on `vendor/pubgrub/src/report.rs`:122 on 2023-11-14 17:26_

Or we could make the reason it's own enum with its own merge semantics, but that requires encoding the set of reasons in the PubGrub crate.

---

_@charliermarsh reviewed on 2023-11-14 18:11_

---

_Review comment by @charliermarsh on `vendor/pubgrub/src/internal/incompatibility.rs`:111 on 2023-11-14 18:11_

The line breaks here make me read this like a haiku :joy:

---

_@zanieb reviewed on 2023-11-14 18:18_

---

_Review comment by @zanieb on `vendor/pubgrub/src/internal/incompatibility.rs`:111 on 2023-11-14 18:18_

Me too haha I just kept the existing style but it's funny

---

_Comment by @zanieb on 2023-11-15 00:27_

I don't understand why the numpy test is failing.

I'm going to look into upstreaming these changes, but I don't think it needs to block us here.

---

_Marked ready for review by @zanieb on 2023-11-15 00:27_

---

_Comment by @konstin on 2023-11-15 08:54_

#426 Fixes CI (See #427)

---

_@charliermarsh approved on 2023-11-15 16:49_

---

_Comment by @konstin on 2023-11-15 18:55_

(fixed the merge conflict i created)

---

_Comment by @zanieb on 2023-11-15 19:48_

An upstream pull request has been opened for discussion at https://github.com/pubgrub-rs/pubgrub/pull/153.

---

_Comment by @zanieb on 2023-11-16 14:50_

While I intend to continue work upstream, I'd like to merge this for my own sanity?

Perhaps we should create a PubGrub fork instead so we can track these changes?

---

_Comment by @charliermarsh on 2023-11-16 18:04_

👍 On setting up our own fork as discussed in our 1:1.

---

_@zanieb reviewed on 2023-11-16 19:58_

---

_Review comment by @zanieb on `Cargo.toml`:51 on 2023-11-16 19:58_

https://github.com/zanieb/pubgrub/pull/4

---

_@zanieb reviewed on 2023-11-16 19:58_

---

_Review comment by @zanieb on `vendor/pubgrub/src/report.rs`:192 on 2023-11-16 19:58_

See https://github.com/pubgrub-rs/pubgrub/pull/153

---

_Merged by @zanieb on 2023-11-16 20:02_

---

_Closed by @zanieb on 2023-11-16 20:02_

---

_Branch deleted on 2023-11-16 20:02_

---
