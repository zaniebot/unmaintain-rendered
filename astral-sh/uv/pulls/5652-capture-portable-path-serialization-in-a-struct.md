```yaml
number: 5652
title: Capture portable path serialization in a struct
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/portable
created_at: 2024-07-31T13:56:52Z
updated_at: 2024-07-31T16:00:38Z
url: https://github.com/astral-sh/uv/pull/5652
synced_at: 2026-01-10T13:37:23Z
```

# Capture portable path serialization in a struct

---

_Pull request opened by @charliermarsh on 2024-07-31 13:56_

## Summary

I need to reuse this in #5494, so want to abstract it out and make it reusable.

---

_Label `internal` added by @charliermarsh on 2024-07-31 13:56_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-07-31 13:56_

---

_Review requested from @konstin by @charliermarsh on 2024-07-31 13:57_

---

_Marked ready for review by @charliermarsh on 2024-07-31 13:57_

---

_@zanieb approved on 2024-07-31 14:06_

I think there a couple uses of this pattern in workspaces and tools, presume you'll be updating those though.

---

_Comment by @charliermarsh on 2024-07-31 14:18_

@zanieb -- Thanks, updated! I missed those.

---

_Review comment by @konstin on `crates/uv-resolver/src/lock.rs`:1707 on 2024-07-31 15:13_

Commented out code

---

_@konstin approved on 2024-07-31 15:14_

---

_Review comment by @BurntSushi on `crates/uv-fs/src/path.rs`:311 on 2024-07-31 15:34_

I think this is mostly fine. But the big caveat here is that this implementation assumes the file path is valid UTF-8. We generally already assume this everywhere in `uv`, and undoing that assumption would be annoying (but possible). In any case, because we assume UTF-8 everywhere already, assuming it here seems fine too. But I would definitely put a mention of it in the docs. (And that somewhat makes the name "portable" a little suspicious, although I see why you chose that name.)

In terms of the actual "wrapper type" mechanics, I think the main alternative in this design space would be to define `PortablePath` as a DST like `Path` is. But doing so requires `unsafe` and IDK if it's worth doing here. The nice thing about the DST approach is that the lifetime specifier gets elevated to the borrow and removed from the type. So you'd have `&'a PortablePath` instead of `PortablePath<'a>`. That in turn makes it possible to implement certain traits like `AsRef<PortablePath>`. You could even have a `impl AsRef<PortablePath> for Path`.

The extent to which the "wrapper type" mechanics matters depends a lot on how it's intended to use this type. If it's mostly just intended to be used in a few places, then keeping its API slim and devoted to the integration points makes sense. If instead we wanted to use this everywhere we use `Path`, then we'd want something more elaborate probably.

---

_@BurntSushi reviewed on 2024-07-31 15:34_

---

_@AlexWaygood reviewed on 2024-07-31 15:39_

---

_Review comment by @AlexWaygood on `crates/uv-fs/src/path.rs`:311 on 2024-07-31 15:39_

For some more elaborate examples that use DSTs like `Path` (and therefore require `#repr(transparent)` and unsafe code in the constructors), you can check out what we've been doing in red-knot:
- https://github.com/astral-sh/ruff/blob/main/crates/ruff_db/src/system/path.rs
- https://github.com/astral-sh/ruff/blob/main/crates/ruff_db/src/vendored/path.rs

(I've no idea if that's the way you want to go or not)

---

_Review comment by @charliermarsh on `crates/uv-fs/src/path.rs`:311 on 2024-07-31 15:46_

Thank you!

---

_@charliermarsh reviewed on 2024-07-31 15:46_

---

_@AlexWaygood reviewed on 2024-07-31 15:50_

---

_Review comment by @AlexWaygood on `crates/uv-fs/src/path.rs`:311 on 2024-07-31 15:50_

Oh, and with regards to the UTF-8 problem that @BurntSushi mentions above -- in red-knot we make our `Path` and `PathBuf` structs wrappers around `camino::Utf8Path` and `camino::Utf8PathBuf` (respectively), rather than `std::path::Path` and `std::path::PathBuf`.

---

_@charliermarsh reviewed on 2024-07-31 15:51_

---

_Review comment by @charliermarsh on `crates/uv-fs/src/path.rs`:311 on 2024-07-31 15:51_

Yeah we need to do something similar at some point.

---

_Merged by @charliermarsh on 2024-07-31 16:00_

---

_Closed by @charliermarsh on 2024-07-31 16:00_

---

_Branch deleted on 2024-07-31 16:00_

---
