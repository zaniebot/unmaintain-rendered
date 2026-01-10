```yaml
number: 1197
title: "puffin-client: simplify use of http-cache-semantics"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/cache-client-simplification
created_at: 2024-01-30T23:12:25Z
updated_at: 2024-01-30T23:20:45Z
url: https://github.com/astral-sh/uv/pull/1197
synced_at: 2026-01-10T15:39:03Z
```

# puffin-client: simplify use of http-cache-semantics

---

_Pull request opened by @BurntSushi on 2024-01-30 23:12_

The `http-cache-semantics` crate is polymorphic on the types of requests
and responses it accepts. We had previously been explicitly converting
between `http` and `reqwest` types, but this isn't necessary. We can
provide impls of the traits in `http-cache-semantics` for `reqwest`'s
types (via a wrapper). This saves us from the awkward request/response
type conversions.

While this does clone the request, this is:

1. Not new. We were previously cloning the request to do the conversion.
2. An artifact (I believe) of http-cache-semantics API. (It kind of
   seems like an API bug to me?)

There is also a little bit of messiness around inter-operating between
http::uri::Uri and url::Url. But overall shouldn't be a big deal.


---

_Review requested from @konstin by @BurntSushi on 2024-01-30 23:12_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-01-30 23:12_

---

_@charliermarsh approved on 2024-01-30 23:13_

---

_@konstin approved on 2024-01-30 23:18_

Neat, i didn't realize this was possible!

---

_Merged by @BurntSushi on 2024-01-30 23:20_

---

_Closed by @BurntSushi on 2024-01-30 23:20_

---

_Branch deleted on 2024-01-30 23:20_

---
