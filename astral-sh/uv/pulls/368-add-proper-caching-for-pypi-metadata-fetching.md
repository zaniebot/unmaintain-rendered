```yaml
number: 368
title: Add proper caching for pypi metadata fetching kinds
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: cache-responses
created_at: 2023-11-08T20:58:20Z
updated_at: 2023-11-10T11:03:42Z
url: https://github.com/astral-sh/uv/pull/368
synced_at: 2026-01-10T15:50:28Z
```

# Add proper caching for pypi metadata fetching kinds

---

_Pull request opened by @konstin on 2023-11-08 20:58_

I intend this to become the main form of caching for puffin: You can make http requests, you tranform the data to what you really need, you have control over the cache key, and the cache is always json (or anything else much faster we want to replace it with as long as it's serde!)

---

_@charliermarsh reviewed on 2023-11-09 00:47_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/client.rs`:216 on 2023-11-09 00:47_

So in theory we could change this to be a "thin" type rather than the full `Metadata21`?

---

_@konstin reviewed on 2023-11-09 10:29_

---

_Review comment by @konstin on `crates/puffin-client/src/client.rs`:216 on 2023-11-09 10:29_

Yes, that's very much intended here

---

_@konstin reviewed on 2023-11-09 10:33_

---

_Review comment by @konstin on `crates/puffin-cache/src/digest.rs`:5 on 2023-11-09 10:33_

This fixes a `cargo doc` warning


---

_Marked ready for review by @konstin on 2023-11-09 10:37_

---

_Review comment by @charliermarsh on `Cargo.toml`:17 on 2023-11-10 02:19_

Can we use a dedicated ref instead of a branch, for immtuability?

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/digest.rs`:5 on 2023-11-10 02:19_

Hmm, that's strange?

---

_Review comment by @charliermarsh on `crates/puffin-dev/src/wheel_metadata.rs`:36 on 2023-11-10 02:21_

Is this a behavior change, or you just find it clearer?

---

_Review comment by @charliermarsh on `scripts/benchmarks/requirements/mst.in`:1 on 2023-11-10 02:21_

(Nit: maybe spell out the full name in the filename, for consistency?)

---

_@charliermarsh approved on 2023-11-10 02:21_

---

_Comment by @charliermarsh on 2023-11-10 02:21_

Nice work.

---

_@konstin reviewed on 2023-11-10 09:23_

---

_Review comment by @konstin on `crates/puffin-cache/src/digest.rs`:5 on 2023-11-10 09:23_



It complained that the linked symbol is private


---

_@konstin reviewed on 2023-11-10 09:56_

---

_Review comment by @konstin on `crates/puffin-dev/src/wheel_metadata.rs`:36 on 2023-11-10 09:56_

I'd say it makes `RegistryClient` consistent with the rest of the codebase, which already uses a non-optional cache dir

---

_Review comment by @konstin on `Cargo.toml`:17 on 2023-11-10 10:54_

Done, will update when it's merged to upstream

---

_@konstin reviewed on 2023-11-10 10:54_

---

_Merged by @konstin on 2023-11-10 11:03_

---

_Closed by @konstin on 2023-11-10 11:03_

---

_Branch deleted on 2023-11-10 11:03_

---
