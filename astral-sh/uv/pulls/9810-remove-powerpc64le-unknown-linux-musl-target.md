```yaml
number: 9810
title: "Remove `powerpc64le-unknown-linux-musl` target"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/ppc
created_at: 2024-12-11T13:57:04Z
updated_at: 2024-12-11T14:30:53Z
url: https://github.com/astral-sh/uv/pull/9810
synced_at: 2026-01-12T16:08:59Z
```

# Remove `powerpc64le-unknown-linux-musl` target

---

_@charliermarsh_

## Summary

This is blocking the release (#9793). We seem to have hit some sort of limit that's causing builds to fail on this target. It's a Tier 3 Rust target with _unknown_ (???) `std` support (see the question mark [here](https://doc.rust-lang.org/rustc/platform-support.html)).

---

_Renamed from "Remove powerpc64le-unknown-linux-musl target" to "Remove `powerpc64le-unknown-linux-musl` target" by @charliermarsh on 2024-12-11 13:57_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-12-11 13:58_

---

_Review requested from @zanieb by @charliermarsh on 2024-12-11 13:58_

---

_Review requested from @konstin by @charliermarsh on 2024-12-11 13:58_

---

_Marked ready for review by @charliermarsh on 2024-12-11 13:58_

---

_Comment by @charliermarsh on 2024-12-11 13:58_

It looks like we don't even include these in our cargo-dist release?

---

_@zanieb approved on 2024-12-11 14:25_

---

_Merged by @charliermarsh on 2024-12-11 14:30_

---

_Closed by @charliermarsh on 2024-12-11 14:30_

---

_Branch deleted on 2024-12-11 14:30_

---
