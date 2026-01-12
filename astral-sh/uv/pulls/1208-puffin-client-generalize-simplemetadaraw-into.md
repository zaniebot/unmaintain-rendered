```yaml
number: 1208
title: "puffin-client: generalize SimpleMetadaRaw into OwnedArchive<A>"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
assignees: []
merged: true
base: main
head: ag/rkyv-generic-view
created_at: 2024-01-31T16:26:51Z
updated_at: 2024-02-02T17:01:38Z
url: https://github.com/astral-sh/uv/pull/1208
synced_at: 2026-01-12T16:04:30Z
```

# puffin-client: generalize SimpleMetadaRaw into OwnedArchive<A>

---

_@BurntSushi_

It turns out that the pattern I coded up for SimpleMetadataRaw is
generally useful when working with rkyv. This commit makes it generic by
supporting any type that implements rkyv's traits, and makes a few
simplifying assumptions by picking a concrete serializer, validator and
deserializer. In effect, this lets use own any archived value.

We also rejigger the API a little bit and double-down on
`OwnedArchive<A>` just being a owned wrapper for `Archived<A>`. Namely,
we implement `Deref` and turn its inherent methods into methods that
require fully qualified syntax. (As is standard for things that
implement `Deref` to avoid ambiguity with the deref target's methods.)

(This PR also makes a couple small simplifications to our custom rkyv
serializer since we no longer need to use it directly. We do still need
to name the type in trait bounds, so it has to be public.)


---

_Review requested from @konstin by @BurntSushi on 2024-01-31 16:27_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-01-31 16:27_

---

_@charliermarsh approved on 2024-01-31 16:42_

Makes sense to me.

---

_Merged by @BurntSushi on 2024-01-31 16:56_

---

_Closed by @BurntSushi on 2024-01-31 16:56_

---

_Branch deleted on 2024-01-31 16:56_

---

_Label `internal` added by @zanieb on 2024-02-02 17:01_

---
