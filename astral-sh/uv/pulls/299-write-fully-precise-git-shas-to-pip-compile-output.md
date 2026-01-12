```yaml
number: 299
title: "Write fully-precise Git SHAs to `pip-compile` output"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/refresh
created_at: 2023-11-02T20:05:07Z
updated_at: 2023-11-03T16:26:59Z
url: https://github.com/astral-sh/uv/pull/299
synced_at: 2026-01-12T16:03:51Z
```

# Write fully-precise Git SHAs to `pip-compile` output

---

_@charliermarsh_

This PR adds a mechanism by which we can ensure that we _always_ try to refresh Git dependencies when resolving; further, we now write the fully resolved SHA to the "lockfile". However, nothing in the code _assumes_ we do this, so the installer will remain agnostic to this behavior.

The specific approach taken here is minimally invasive. Specifically, when we try to fetch a source distribution, we check if it's a Git dependency; if it is, we fetch, and return the exact SHA, which we then map back to a new URL. In the resolver, we keep track of URL "redirects", and then we use the redirect (1) for the actual source distribution building, and (2) when writing back out to the lockfile. As such, none of the types outside of the resolver change at all, since we're just mapping `RemoteDistribution` to `RemoteDistribution`, but swapping out the internal URLs.

There are some inefficiencies here since, e.g., we do the Git fetch, send back the "precise" URL, then a moment later, do a Git checkout of that URL (which will be _mostly_ a no-op -- since we have a full SHA, we don't have to fetch anything, but we _do_ check back on disk to see if the SHA is still checked out). A more efficient approach would be to return the path to the checked-out revision when we do this conversion to a "precise" URL, since we'd then only interact with the Git repo exactly once. But this runs the risk that the checked-out SHA changes between the time we make the "precise" URL and the time we build the source distribution.

Closes #286.

---

_Comment by @zanieb on 2023-11-02 20:18_

This looks nice!

> But this runs the risk that the checked-out SHA changes between the time we make the "precise" URL and the time we build the source distribution.

Is there a case where this can happen?

---

_Comment by @charliermarsh on 2023-11-02 20:20_

No, it shouldn't happen currently, and I may refactor to take advantage of that, but the separation ended up being a bit simpler for now.

---

_Marked ready for review by @charliermarsh on 2023-11-02 20:35_

---

_@charliermarsh reviewed on 2023-11-02 20:36_

---

_Review comment by @charliermarsh on `crates/puffin-git/src/git.rs`:927 on 2023-11-02 20:36_

Apparently this fetch doesn't work for short commits...? It may actually be a bug in Cargo? I think you only hit this if the GitHub fast path fails, and that only fails if you get rate limited.

---

_Review requested from @zanieb by @charliermarsh on 2023-11-02 20:37_

---

_Review requested from @konstin by @charliermarsh on 2023-11-02 20:37_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver.rs`:879 on 2023-11-02 20:37_

I hate that this is a wait map, but it needs to be mutably shareable because it's used in the channel stream.

---

_@charliermarsh reviewed on 2023-11-02 20:37_

---

_Review comment by @konstin on `crates/puffin-git/src/git.rs`:1285 on 2023-11-03 16:15_

huh?

---

_@konstin approved on 2023-11-03 16:20_

---

_Merged by @charliermarsh on 2023-11-03 16:26_

---

_Closed by @charliermarsh on 2023-11-03 16:26_

---

_Branch deleted on 2023-11-03 16:26_

---
