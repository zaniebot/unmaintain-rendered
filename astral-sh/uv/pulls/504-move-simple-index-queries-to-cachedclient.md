```yaml
number: 504
title: "Move simple index queries to `CachedClient`"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: Move-simple-index-queries-to-CachedClient
created_at: 2023-11-27T13:59:59Z
updated_at: 2023-11-28T00:11:05Z
url: https://github.com/astral-sh/uv/pull/504
synced_at: 2026-01-12T16:03:59Z
```

# Move simple index queries to `CachedClient`

---

_@konstin_

Replaces the usage of `http-cache-reqwest` for simple index queries with our custom cached client, removing `http-cache-reqwest` altogether. 

The new cache paths are `<cache>/simple-v0/<index>/<package_name>.json`. I could not test with a non-pypi index since i'm not aware of any other json indices (jax and torch are both html indices).

In a future step, we can transform the response to be a `HashMap<Version, {source_dists: Vec<(SourceDistFilename, File)>, wheels: Vec<(WheeFilename, File)>}` (independent of python version, this cache is used by all environments together). This should speed up cache deserialization a bit, since we don't need to try source dist and wheel anymore and drop incompatible dists, and it should make building the `VersionMap` simpler. We can speed this up even further by splitting into a version lists and the info for each version. I'm mentioning this because deserialization was a major bottleneck in the rust part of the old python prototype.

Fixes #481

---

_Review comment by @charliermarsh on `crates/puffin-client/src/cached_client.rs`:179 on 2023-11-27 19:31_

Nit: can we expand this message to be more descriptive? I wouldn't know what `Fresh {url}` means in the logs from this alone.

---

_Review comment by @charliermarsh on `crates/puffin-client/src/cached_client.rs`:185 on 2023-11-27 19:31_

Nit: inline `{url}` here and elsewhere.

---

_Review comment by @charliermarsh on `crates/puffin-client/src/cached_client.rs`:179 on 2023-11-27 19:31_

Maybe like: `Found fresh cache response for: {url}`

---

_@charliermarsh approved on 2023-11-27 19:32_

---

_@charliermarsh reviewed on 2023-11-27 19:32_

---

_Review comment by @charliermarsh on `scripts/benchmarks/requirements/jax.in`:1 on 2023-11-27 19:32_

Should we just omit this then? Why check it in if it's broken?

---

_Merged by @charliermarsh on 2023-11-28 00:11_

---

_Closed by @charliermarsh on 2023-11-28 00:11_

---

_Branch deleted on 2023-11-28 00:11_

---
