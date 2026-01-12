```yaml
number: 3351
title: "universal-lock: propagate full set of artifacts for each distribution"
type: issue
state: closed
author: BurntSushi
labels:
  - preview
assignees: []
created_at: 2024-05-03T15:29:29Z
updated_at: 2024-05-15T19:07:30Z
url: https://github.com/astral-sh/uv/issues/3351
synced_at: 2026-01-12T15:58:43Z
```

# universal-lock: propagate full set of artifacts for each distribution

---

_@BurntSushi_

At present, our resolver does not deal with cross platform concerns. Instead, it assumes there is a single fixed set of marker values for the current environment, and makes decisions based on those. Even more, the structure of the code itself is oriented around this assumption.

In this ticket, we'll need to relax one manifestation of that assumption: the collapsing of distribution "simple" metadata down into a single wheel/sdist. Right now, this happens at a fairly low level in our `VersionMap`:

https://github.com/astral-sh/uv/blob/e33ff95575e8165078799548070210947d1cfad0/crates/uv-resolver/src/version_map.rs#L344-L431

That is, when retrieving metadata for a distribution, we almost immediately collapse the metadata down into the single "best" wheel based on the current marker environment. But in a cross platform context, all of the wheels need to make it into the lock file somehow, and thus we can't discard this metadata.

I think the simplest change we can make here is to continue selecting the "best" wheel at this point, but instead of throwing away everything else, a `PrioritizedDist` hangs on to it. And this information probably needs to be threaded through to other downstream types, such as `ResolvedDist` (and of its "built" member types).

---

_Label `preview` added by @BurntSushi on 2024-05-03 15:29_

---

_Closed by @BurntSushi on 2024-05-15 19:07_

---
