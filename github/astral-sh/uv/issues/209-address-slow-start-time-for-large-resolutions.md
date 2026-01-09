---
number: 209
title: Address slow start-time for large resolutions
type: issue
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
created_at: 2023-10-26T19:18:27Z
updated_at: 2023-12-28T16:49:13Z
url: https://github.com/astral-sh/uv/issues/209
synced_at: 2026-01-07T13:12:16-06:00
---

# Address slow start-time for large resolutions

---

_Issue opened by @charliermarsh on 2023-10-26 19:18_

If you try to resolve `./scripts/resolve/pypi_top_8k_flat.txt`, there's like a 1 second delay at the start of the resolution. The issue is that we kick off requests eagerly for all 8,000 packages, and then we wait for the first package in `potential_packages` to be available. If that first package (which isn't guaranteed to match the order of the initial requirements) is deep in the list, we end up waiting a long time.

---

_Label `performance` added by @charliermarsh on 2023-10-26 19:18_

---

_Added to milestone `Initial release` by @charliermarsh on 2023-10-26 19:18_

---

_Comment by @charliermarsh on 2023-10-26 19:22_

I don't know what the right solution is here. Pick packages in the order they were fetched, basically? But I don't know how to implement that.

---

_Comment by @charliermarsh on 2023-10-26 19:27_

Another thing we could explore here is doing a better job loading cached metadata upfront. Like, we could do a blocking operation upfront to ensure we pre-load any cached metadata without going through channels or the reqwest client.

---

_Comment by @charliermarsh on 2023-10-26 19:56_

For 500 packages, if you just try to fetch all the (cached) metadata in a loop upfront, it takes about 600ms on a release build.

---

_Comment by @charliermarsh on 2023-10-26 19:59_

It seems like all the time is spent deserializing and computing SHAs (42% in `sha256::compress256`).

---

_Comment by @konstin on 2023-12-28 14:47_

I tried this again and i get
```
[...] DEBUG puffin_resolver::resolver Adding direct dependency: [..., x 8000]
0.117878s INFO pubgrub::internal::partial_solution add_decision: root @ 0a0.dev0
[...]
0.273812s DEBUG puffin_resolver::resolver Searching for a compatible version of numba (*)
[...] 0.274196s DEBUG puffin_client::cached_client Found fresh response for: [...]
```

with

```
RUST_LOG=debug cargo run --bin puffin -- pip-compile -v ./scripts/popular_packages/pypi_8k_downloads.txt
```

---

_Comment by @charliermarsh on 2023-12-28 16:49_

Let's close this for now, it's not super actionable.

---

_Closed by @charliermarsh on 2023-12-28 16:49_

---
