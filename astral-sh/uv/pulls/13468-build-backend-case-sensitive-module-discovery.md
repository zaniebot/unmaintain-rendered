```yaml
number: 13468
title: " Build backend: Case sensitive module discovery"
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/build-backend-path-casing
created_at: 2025-05-15T13:59:52Z
updated_at: 2025-05-19T06:03:09Z
url: https://github.com/astral-sh/uv/pull/13468
synced_at: 2026-01-12T16:10:41Z
```

#  Build backend: Case sensitive module discovery

---

_@konstin_

We may run on case-sensitive file systems (Linux, generally) or on case-insensitive file systems (Windows, generally), while modules in Python may be lower or upper case. For robustness over filesystem casing, we require an explicit module name for modules with upper case letters.

Fixes #13419


---

_Label `bug` added by @konstin on 2025-05-15 13:59_

---

_@konstin reviewed on 2025-05-15 14:00_

---

_Review comment by @konstin on `crates/uv-globfilter/src/portable_glob.rs`:98 on 2025-05-15 14:00_

This is the core change, the other changes are in support of that.

---

_Review comment by @konstin on `crates/uv-build-backend/src/lib.rs`:243 on 2025-05-15 14:01_

Centralized here for normalization

---

_@konstin reviewed on 2025-05-15 14:01_

---

_Review requested from @BurntSushi by @konstin on 2025-05-15 14:01_

---

_@BurntSushi approved on 2025-05-15 16:33_

I think this LGTM, but my main concern here is doing case insensitive globbing on _all_ file systems. Couldn't that be unexpected behavior?

To try and answer my own question, I think the idea is that you want the build backend to match the same set of files _regardless_ of platform, so that you get consistency. To that end, always doing case insensitive globbing means you fall to the lowest common denominator, so to speak. With that said, couldn't this be quite surprising for users on file systems that are case sensitive?

And should this behavior be documented somewhere?

---

_Comment by @konstin on 2025-05-15 18:48_

> To try and answer my own question, I think the idea is that you want the build backend to match the same set of files _regardless_ of platform, so that you get consistency. To that end, always doing case insensitive globbing means you fall to the lowest common denominator, so to speak. With that said, couldn't this be quite surprising for users on file systems that are case sensitive?

Are there alternatives that have good behavior across platforms? My primary example would be a module called `PIL`, and I want to build that on Linux, Windows and Mac. Are there any behaviors that can reliably expect across platforms? Part of the problem is that I'm trying to match a path in a directory, instead of opening it; this allows `pil` to automatically find `PIL`, because the user may have set the package name to `Pil` and we normalized it away. Maybe we shouldn't do that and always force configuration for names with uppercase letters?

> And should this behavior be documented somewhere?

Will do.

---

_Comment by @konstin on 2025-05-16 09:35_

I switched strategies, uppercase names need to be declared explicitly in `tool.uv.module-name`.

---

_@konstin reviewed on 2025-05-16 09:35_

---

_Review comment by @konstin on `docs/configuration/build-backend.md`:43 on 2025-05-16 09:35_

This section will become "Modules and Namespaces" once implemented.

---

_@BurntSushi approved on 2025-05-16 12:22_

Nice! I think this seems better to me.

---

_Renamed from " Build backend: Case insensitive module discovery" to " Build backend: Case sensitive module discovery" by @konstin on 2025-05-16 12:25_

---

_Merged by @konstin on 2025-05-16 12:25_

---

_Closed by @konstin on 2025-05-16 12:25_

---

_Branch deleted on 2025-05-16 12:25_

---
