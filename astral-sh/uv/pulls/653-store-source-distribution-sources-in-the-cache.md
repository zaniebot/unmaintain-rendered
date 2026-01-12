```yaml
number: 653
title: Store source distribution sources in the cache
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/sdist
created_at: 2023-12-15T03:46:52Z
updated_at: 2023-12-15T17:19:35Z
url: https://github.com/astral-sh/uv/pull/653
synced_at: 2026-01-12T16:04:06Z
```

# Store source distribution sources in the cache

---

_@charliermarsh_

## Summary

This PR modifies `source_dist.rs` to store source distributions (from remote URLs) in the cache. The cache structure for registries now looks like:

<img width="1053" alt="Screen Shot 2023-12-14 at 10 43 43 PM" src="https://github.com/astral-sh/puffin/assets/1309177/3c2dbf6b-5926-41f2-b69b-74031741aba8">

(I will update the docs prior to merging, if approved.)

The benefit here is that we can reuse the source distribution (avoid download + unzipping it) if we need to build multiple wheels. In the future, it will be even more relevant, since we'll need to reuse the source distribution to support https://github.com/astral-sh/puffin/issues/599.

I also included some misc. refactors to DRY up repeated operations and add some more abstraction to `source_dist.rs`.

---

_Review requested from @konstin by @charliermarsh on 2023-12-15 03:47_

---

_Label `enhancement` added by @charliermarsh on 2023-12-15 03:47_

---

_Review comment by @konstin on `crates/puffin-distribution/src/source_dist.rs`:559 on 2023-12-15 08:26_

```suggestion
        let cache_file = cache_entry.path();
        if cache_file.is_dir() {
```

---

_@konstin approved on 2023-12-15 08:30_

Btw i think we should break metadata.json into `cache_info.json` and individual wheel metadata (as in the wheel cache), i thought this would it would be easier for caching to have it all in one file but it isn't

---

_Merged by @charliermarsh on 2023-12-15 17:19_

---

_Closed by @charliermarsh on 2023-12-15 17:19_

---

_Branch deleted on 2023-12-15 17:19_

---
