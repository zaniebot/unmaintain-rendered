```yaml
number: 1150
title: "puffin-client: add SimpleMetadataRaw"
type: pull_request
state: merged
author: BurntSushi
labels:
  - performance
  - internal
assignees: []
merged: true
base: main
head: ag/rkyv-simple-metadata-raw
created_at: 2024-01-27T13:28:40Z
updated_at: 2024-01-29T14:37:07Z
url: https://github.com/astral-sh/uv/pull/1150
synced_at: 2026-01-10T15:39:03Z
```

# puffin-client: add SimpleMetadataRaw

---

_Pull request opened by @BurntSushi on 2024-01-27 13:28_

This adds what is effectively an owned wrapper around
`Archived<SimpleMetadata>`. Normally, an `Archived<SimpleMetadata>`
has to be used behind a pointer (since it has a lifetime
attached to its underlying byte buffer), but we create a
wrapper around it that owns the underlying buffer and provides
free access to the archived type.

This in effect creates an anchor point for the archived type
and lets us pass it around easily. (There has to be an anchor
point for it somewhere.)

An alternative to this approach would be to store it as a file
backed memory map. But in practice, we're dealing with small
files, and just reading them on to the heap is likely to be
faster. (Memory maps also have wildly different perf characteristics
across platforms.)

Note that this commit just defines the type. It isn't actually
used anywhere yet.


---

_Review requested from @charliermarsh by @BurntSushi on 2024-01-27 13:28_

---

_Label `internal` added by @BurntSushi on 2024-01-27 13:29_

---

_Label `performance` added by @BurntSushi on 2024-01-27 13:29_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/rkyvutil.rs`:15 on 2024-01-28 15:34_

I feel dumb asking this, but why `/*!` instead of `//!`?

---

_@charliermarsh approved on 2024-01-29 01:14_

---

_@BurntSushi reviewed on 2024-01-29 14:36_

---

_Review comment by @BurntSushi on `crates/puffin-client/src/rkyvutil.rs`:15 on 2024-01-29 14:36_

No particularly strong reason. If rustfmt could normalize everything to `//!` I'd have no issue with that hah. Just a stylistic thing that I've always used. I find not having `//!` at the beginning of each line nicer, especially for module docs which tend to be a lot longer.

---

_Merged by @BurntSushi on 2024-01-29 14:37_

---

_Closed by @BurntSushi on 2024-01-29 14:37_

---

_Branch deleted on 2024-01-29 14:37_

---
