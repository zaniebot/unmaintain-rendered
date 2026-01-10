```yaml
number: 507
title: "Introduce `Cache`, `CacheBucket` and `CacheEntry`"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: cache_abstraction_
created_at: 2023-11-28T13:41:19Z
updated_at: 2023-11-28T17:12:50Z
url: https://github.com/astral-sh/uv/pull/507
synced_at: 2026-01-10T15:44:44Z
```

# Introduce `Cache`, `CacheBucket` and `CacheEntry`

---

_Pull request opened by @konstin on 2023-11-28 13:41_

This is mostly a mechanical refactor that moves 80% of our code to the same cache abstraction.

It introduces cache `Cache`, which abstracts away the path of the cache and the temp dir drop and is passed throughout the codebase. To get a specific cache bucket, you need to requests your `CacheBucket` from `Cache`. `CacheBucket` is the centralizes the names of all cache buckets, moving them away from the string constants spread throughout the crates.

Specifically for working with the `CachedClient`, there is a `CacheEntry`. I'm not sure yet if that is a strict improvement over `cache_dir: PathBuf, cache_file: String`, i may have to rotate that later.

The interpreter cache moved into `interpreter-v0`.

We can use the `CacheBucket` page to document the cache structure in each bucket:

![image](https://github.com/astral-sh/puffin/assets/6826232/b023fdfb-e34d-4c2d-8663-b5f73937a539)


---

_Label `internal` added by @konstin on 2023-11-28 13:41_

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/lib.rs`:24 on 2023-11-28 16:59_

Nit: `derive(Debug)` for all structs that can.

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/lib.rs`:189 on 2023-11-28 17:02_

We may want to do another pass on some of these names (e.g. archives vs. built wheels vs. wheels), but perhaps as a separate PR.

---

_@charliermarsh approved on 2023-11-28 17:02_

Great.

---

_Comment by @charliermarsh on 2023-11-28 17:07_

Going to merge your stuff so I have up-to-date repo on the plane.

---

_Merged by @charliermarsh on 2023-11-28 17:11_

---

_Closed by @charliermarsh on 2023-11-28 17:11_

---

_Branch deleted on 2023-11-28 17:11_

---

_@konstin reviewed on 2023-11-28 17:12_

---

_Review comment by @konstin on `crates/puffin-cache/src/lib.rs`:189 on 2023-11-28 17:12_

Yes i also thought about changing them but didn't want to increase the churn more here

---
