```yaml
number: 464
title: Start to move more basic tasks into puffin-dispatch
type: pull_request
state: closed
author: konstin
labels: []
assignees: []
base: main
head: konstin/deduplicate
created_at: 2023-11-20T09:02:12Z
updated_at: 2023-11-28T16:27:22Z
url: https://github.com/astral-sh/uv/pull/464
synced_at: 2026-01-10T15:44:44Z
```

# Start to move more basic tasks into puffin-dispatch

---

_Pull request opened by @konstin on 2023-11-20 09:02_

Cherry-picked 67dd5b321d9ae99c140ea35c807eb656a499cb83 from #460, avoiding the conflicts from de1d1223e0152d5ec285cf8ea70fdc1a47f86e5f (which should be on main now anyway)

---

## Summary

This PR unifies the behavior that lived in the resolver's `distribution` crates with the behaviors that were spread between the various structs in the installer crate into a single `Fetcher` struct that is intended to manage all interactions with distributions. Specifically, the interface of this struct is such that it can access distribution metadata, download distributions, return those downloads, etc., all with a common cache.

Overall, this is mostly just DRYing up code that was repeated between the two crates, and putting it behind a reasonable shared interface.

---

_Review comment by @konstin on `crates/puffin-distribution/src/download.rs`:119 on 2023-11-20 09:07_

We should give this a proper error type with context on which wheel is invalid

---

_Review comment by @konstin on `crates/puffin-distribution/src/fetcher.rs`:79 on 2023-11-20 09:08_

The part from here downwards i'll have to change a lot

---

_@konstin reviewed on 2023-11-20 09:10_

---

_Closed by @konstin on 2023-11-20 11:02_

---

_Branch deleted on 2023-11-28 16:27_

---
