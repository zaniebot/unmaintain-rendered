```yaml
number: 1098
title: "puffin-client: decouple serde from cached client"
type: pull_request
state: closed
author: BurntSushi
labels: []
assignees: []
draft: true
base: main
head: ag/decouple-cached-client
created_at: 2024-01-25T16:34:36Z
updated_at: 2024-02-16T21:37:46Z
url: https://github.com/astral-sh/uv/pull/1098
synced_at: 2026-01-10T15:33:24Z
```

# puffin-client: decouple serde from cached client

---

_Pull request opened by @BurntSushi on 2024-01-25 16:34_

The motivation for this change is that I'd like to use something that
*isn't* Serde to manage serialization of cached data. Simultaneously,
we'd like to keep existing uses of Serde still working. So this change
does a bit of surgery to a `CachedClient` to remove the coupling with
Serde's serialization traits by introducing a new trait that abstracts
over the Serde operations we need.

We do one other little change here, which is to make deserialization be
performed in two steps: one for the envelope and then another for the
actual data. This is non-ideal for performance, but is necessary for now
I think in order to make zero-copy deserialization work. Namely, the
envelope contains a bunch of HTTP related cache data from an external
crate, and it is not possible to implement the necessary traits to make
it zero-copy deserializeable. So to work around that, we make the
envelope something that stands on its own and contains the raw bytes of
the data we're caching. It is likely that we will eventually want to
remove this two-step serialization process, but to do so and
simultaneously support zero-copy will likely require forking the
`http-cache-semantics` crate (or convincing them to optionally support
`rkyv`).


---

_Closed by @BurntSushi on 2024-02-16 21:37_

---

_Branch deleted on 2024-02-16 21:37_

---
