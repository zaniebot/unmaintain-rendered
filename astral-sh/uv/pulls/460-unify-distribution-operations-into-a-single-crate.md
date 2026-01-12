```yaml
number: 460
title: Unify distribution operations into a single crate
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/deduplicate
created_at: 2023-11-19T19:44:16Z
updated_at: 2023-11-20T11:22:54Z
url: https://github.com/astral-sh/uv/pull/460
synced_at: 2026-01-12T16:03:58Z
```

# Unify distribution operations into a single crate

---

_@charliermarsh_

## Summary

This PR unifies the behavior that lived in the resolver's `distribution` crates with the behaviors that were spread between the various structs in the installer crate into a single `Fetcher` struct that is intended to manage all interactions with distributions. Specifically, the interface of this struct is such that it can access distribution metadata, download distributions, return those downloads, etc., all with a common cache.

Overall, this is mostly just DRYing up code that was repeated between the two crates, and putting it behind a reasonable shared interface.

---

_Review requested from @konstin by @charliermarsh on 2023-11-19 20:09_

---

_Label `internal` added by @charliermarsh on 2023-11-19 20:09_

---

_Marked ready for review by @charliermarsh on 2023-11-19 20:09_

---

_Comment by @charliermarsh on 2023-11-19 20:17_

@konstin - Hopefully not too much overlap with your open PRs. One thing this doesn't yet do is handle the behavior in `unzipper.rs`, which also means that the _unzipped wheel_ caching isn't centralized in here. But it does centralize the behavior for downloading distributions, building source distributions, and accessing distribution metadata.

---

_Comment by @konstin on 2023-11-20 09:11_

Git is really confused, if i cherry pick the second commit i get https://github.com/astral-sh/puffin/pull/464#pullrequestreview-1739258474 which looks good. I did `git reset --hard main && git cherry-pick 67dd5b321d9ae99c140ea35c807eb656a499cb83`, but i don't want to mess up your branch.

---

_Comment by @charliermarsh on 2023-11-20 10:04_

I’ll rebase this shortly.

---

_Comment by @charliermarsh on 2023-11-20 11:01_

\cc @konstin 

---

_Review comment by @konstin on `crates/puffin-distribution/src/download.rs`:119 on 2023-11-20 11:02_

We should give this a proper error type with context on which wheel is invalid


---

_Review comment by @konstin on `crates/puffin-distribution/src/fetcher.rs`:79 on 2023-11-20 11:02_

The part from here downwards i'll have to change a lot


---

_@konstin approved on 2023-11-20 11:02_

---

_@charliermarsh reviewed on 2023-11-20 11:10_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/fetcher.rs`:79 on 2023-11-20 11:10_

Like `fetch_dist`? Yeah I assumed they would change quite a bit, though now they're at least centralized here rather than spread and duplicated between three or four structs.

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/download.rs`:119 on 2023-11-20 11:22_

Mid-way through this but now deciding I'll put it up as a separate PR.

---

_@charliermarsh reviewed on 2023-11-20 11:22_

---

_Merged by @charliermarsh on 2023-11-20 11:22_

---

_Closed by @charliermarsh on 2023-11-20 11:22_

---

_Branch deleted on 2023-11-20 11:22_

---
