```yaml
number: 301
title: Fetch wheel metadata by async range requests on the remote wheel
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: async-range-requests
created_at: 2023-11-02T23:01:56Z
updated_at: 2024-02-14T04:32:12Z
url: https://github.com/astral-sh/uv/pull/301
synced_at: 2026-01-10T15:33:24Z
```

# Fetch wheel metadata by async range requests on the remote wheel

---

_Pull request opened by @konstin on 2023-11-02 23:01_

Use range requests and async zip to extract the METADATA file from a remote wheel.

We currently only cache when the remote says the remote declares the resource as immutable, see https://github.com/06chaynes/http-cache/issues/57 and  https://github.com/baszalmstra/async_http_range_reader/pull/1 . The cache is stored as json with the description omitted, this improve cache deserialization performance.


---

_Renamed from "Async range requests" to "Fetch wheel metadata by async range requests on the remote wheel" by @konstin on 2023-11-03 12:35_

---

_Comment by @charliermarsh on 2023-11-04 13:49_

@konstin - Is this ready for review?

---

_@konstin reviewed on 2023-11-05 22:17_

---

_Review comment by @konstin on `crates/install-wheel-rs/src/lib.rs`:77 on 2023-11-05 22:17_

I expect we'll have to reuse this, at least for async zip and unpacked to the fs

---

_@konstin reviewed on 2023-11-05 22:22_

---

_Review comment by @konstin on `crates/puffin-client/src/client.rs`:261 on 2023-11-05 22:22_

This branch exists but has not been given care since i much rather hope that registries/file hosts support range requests.

---

_Marked ready for review by @konstin on 2023-11-05 22:23_

---

_Comment by @konstin on 2023-11-05 22:24_

It had a branch and tests missing, now it's ready

---

_Review comment by @charliermarsh on `crates/puffin-client/src/client.rs`:214 on 2023-11-06 00:04_

Can we put this comment within the `else`?

---

_@charliermarsh reviewed on 2023-11-06 00:04_

---

_@charliermarsh reviewed on 2023-11-06 00:05_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/client.rs`:251 on 2023-11-06 00:05_

Let's remove this for now -- we should revisit this holistically.

---

_@charliermarsh reviewed on 2023-11-06 00:07_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/client.rs`:251 on 2023-11-06 00:07_

E.g., I think it could be confusing that the metadata is sometimes complete and sometimes incomplete depending on where it comes from. I'd rather store the whole thing, and then change all representations at once.

---

_Review comment by @charliermarsh on `crates/puffin-client/src/error.rs`:53 on 2023-11-06 00:08_

Can you either remove this comment or fold it into the error message? As a reader I don't totally understand what it's saying or why it's relevant.

---

_@charliermarsh reviewed on 2023-11-06 00:08_

---

_Review comment by @charliermarsh on `Cargo.toml`:17 on 2023-11-06 00:10_

It looks like this is published, can we remove the Git dep now?

---

_@charliermarsh reviewed on 2023-11-06 00:10_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/client.rs`:238 on 2023-11-06 00:11_

Sort of wish we could just pass down our cached HTTP client to the async request crate, then we wouldn't need a custom cache implementation (nor would we need to expose the raw client). But I guess the thinking is that we'll need to do this anyway at some point?

---

_@charliermarsh reviewed on 2023-11-06 00:11_

---

_@charliermarsh reviewed on 2023-11-06 00:12_

Generally looks good! A few small comments, and I'd prefer to move the license attribution directly to the source.

---

_@konstin reviewed on 2023-11-06 09:46_

---

_Review comment by @konstin on `Cargo.toml`:17 on 2023-11-06 09:46_

It's not published, i've removed the wrong version

---

_@konstin reviewed on 2023-11-06 09:49_

---

_Review comment by @konstin on `crates/puffin-client/src/client.rs`:238 on 2023-11-06 09:49_

I wouldn't cache those range requests, just the metadata we need eventually (which is even less than the `Metadata21` we have atm). I found deserialization being the major performance bottleneck in the other prototype (even though the deserialization was in rust and the resolver was in python), so i try to cache as minimal data as possible

---

_@charliermarsh approved on 2023-11-06 14:04_

---

_@charliermarsh reviewed on 2023-11-06 14:04_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/client.rs`:238 on 2023-11-06 14:04_

I agree but I'd rather have a consistent caching strategy, and then change over to a _better_ consistent caching strategy, rather than use separate strategies for separate requests. But it's not worth blocking on.

---

_Merged by @konstin on 2023-11-06 14:06_

---

_Closed by @konstin on 2023-11-06 14:06_

---

_Branch deleted on 2023-11-06 14:06_

---
