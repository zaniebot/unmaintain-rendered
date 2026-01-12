```yaml
number: 5654
title: "uv-client: switch heuristic freshness lifetime to hard-coded value"
type: pull_request
state: merged
author: BurntSushi
labels:
  - cache
assignees: []
merged: true
base: main
head: ag/tweak-http-cache-heuristic
created_at: 2024-07-31T14:23:20Z
updated_at: 2024-07-31T15:12:13Z
url: https://github.com/astral-sh/uv/pull/5654
synced_at: 2026-01-12T16:06:56Z
```

# uv-client: switch heuristic freshness lifetime to hard-coded value

---

_@BurntSushi_

The comment in the code explains the bulk of this:

```rust
// We previously computed this heuristic freshness lifetime by
// looking at the difference between the last modified header and
// the response's date header. We then asserted that the cached
// response ought to be "fresh" for 10% of that interval.
//
// It turns out that this can result in very long freshness
// lifetimes[1] that lead to uv caching too aggressively.
//
// Since PyPI sets a max-age of 600 seconds and since we're
// principally just interacting with Python package indices here,
// we just assume a freshness lifetime equal to what PyPI has.
//
// Note though that a better solution here is for the index to
// support proper HTTP caching headers (ideally Cache-Control, but
// Expires also works too, as above).
```

We also remove the `heuristic_percent` field on `CacheConfig`. And since
that's actually part of the cache itself, we bump the simple cache
version.

Finally, we add some more `trace!` calls that should hopefully make
diagnosing issues related to the freshness lifetime a bit easier in the
future.

Fixes #5351


---

_Review requested from @charliermarsh by @BurntSushi on 2024-07-31 14:23_

---

_@charliermarsh approved on 2024-07-31 14:23_

---

_Comment by @charliermarsh on 2024-07-31 14:23_

Thanks @BurntSushi!

---

_Label `cache` added by @charliermarsh on 2024-07-31 14:24_

---

_@zanieb approved on 2024-07-31 15:04_

---

_Merged by @BurntSushi on 2024-07-31 15:12_

---

_Closed by @BurntSushi on 2024-07-31 15:12_

---

_Branch deleted on 2024-07-31 15:12_

---
